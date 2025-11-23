# Pension Calculation Standards

## Skill Metadata
```yaml
type: guardrail
enforcement: block
triggers:
  keywords: [pension, calculation, benefit, retirement, transfer, annuity]
  intent_patterns:
    - "(calculate|compute|determine).*?(pension|benefit|retirement)"
    - "(transfer|commutation|revaluation).*?(value|factor|amount)"
  file_patterns:
    - "**/calculations/**/*.ts"
    - "**/services/pension/**/*.ts"
    - "**/repositories/pension/**/*.ts"
  content_patterns:
    - "calculateBenefit"
    - "pensionAmount"
    - "transferValue"
    - "commutationFactor"
    - "revaluationRate"
```

## Purpose

This skill enforces standards and best practices for pension calculation code in UK pension administration systems. It acts as a **guardrail** to prevent common calculation errors that could result in member underpayment, overpayment, or regulatory non-compliance.

**Enforcement Level**: BLOCK
- Must be used when working with pension calculations
- Will prevent proceeding without following standards
- Can be overridden with @skip-pension-standards comment if justified

## UK Pension Schemes Covered

### 1. Defined Benefit (DB)
Final salary or career average schemes where benefits are predetermined formula-based calculations.

### 2. Defined Contribution (DC)
Money purchase schemes where benefits depend on contributions and investment returns.

### 3. CARE (Career Average Revalued Earnings)
Modern DB variant using career average salary revalued to retirement.

### 4. Hybrid Schemes
Combinations of DB and DC elements.

## Core Calculation Principles

### 1. Precision Requirements

**ALWAYS use appropriate precision for financial calculations:**

```typescript
// ❌ WRONG - JavaScript floating point issues
const pensionAmount = salary * accrualRate * service;

// ✅ CORRECT - Use decimal library for currency
import Decimal from 'decimal.js';
const pensionAmount = new Decimal(salary)
  .times(accrualRate)
  .times(service)
  .toDecimalPlaces(2, Decimal.ROUND_DOWN); // Always round down for member liability
```

**Rules:**
- Use `Decimal.js` or equivalent for all currency calculations
- Round DOWN for liabilities (what we owe members)
- Round UP for contributions (what members owe)
- Store amounts as integers (pence) in database
- Always specify rounding mode explicitly

### 2. Date Handling

**Pension calculations are extremely sensitive to date precision:**

```typescript
// ❌ WRONG - Timezone and precision issues
const retirementDate = new Date('2025-12-31');
const age = retirementDate.getFullYear() - birthDate.getFullYear();

// ✅ CORRECT - Use date library with pension-specific logic
import { parseISO, differenceInDays, addYears } from 'date-fns';

const retirementDate = parseISO('2025-12-31'); // ISO format, no timezone
const age = calculatePensionAge(birthDate, retirementDate); // Use tested helper
const serviceDays = calculatePensionableService(joinDate, leaveDate);
```

**Rules:**
- Always use ISO 8601 date format (YYYY-MM-DD)
- Use `date-fns` or similar library (NOT native Date)
- Calculate ages in complete years (birthday-to-birthday)
- Calculate service in days, then convert to years/months
- Account for leap years
- NRD (Normal Retirement Date) calculations must be exact

### 3. Null/Undefined Handling

**Missing data in pension calculations can cause serious errors:**

```typescript
// ❌ WRONG - Silent failures with nullish values
const pensionAmount = (salary || 0) * accrualRate * service;

// ✅ CORRECT - Explicit validation and error handling
function calculatePension(salary: number, accrualRate: number, service: number): Decimal {
  if (salary === null || salary === undefined) {
    throw new PensionCalculationError('Salary is required for pension calculation');
  }
  if (salary < 0) {
    throw new PensionCalculationError('Salary cannot be negative');
  }
  if (accrualRate === null || accrualRate === undefined) {
    throw new PensionCalculationError('Accrual rate is required');
  }
  if (service === null || service === undefined) {
    throw new PensionCalculationError('Service period is required');
  }

  return new Decimal(salary)
    .times(accrualRate)
    .times(service)
    .toDecimalPlaces(2, Decimal.ROUND_DOWN);
}
```

**Rules:**
- NEVER use default values for missing calculation inputs
- Validate all inputs explicitly
- Throw specific errors (not generic Error)
- Log calculation failures with member context
- Don't proceed with partial data

### 4. Calculation Auditability

**Every pension calculation must be auditable:**

```typescript
// ❌ WRONG - Black box calculation
const pension = calculateFinalSalary(memberId);

// ✅ CORRECT - Audit trail of calculation steps
interface CalculationAuditTrail {
  memberId: string;
  calculationType: 'FINAL_SALARY' | 'CARE' | 'TRANSFER_VALUE';
  inputs: {
    finalSalary?: Decimal;
    accrualRate?: Decimal;
    serviceYears?: Decimal;
    // ... all inputs
  };
  steps: Array<{
    step: string;
    calculation: string;
    result: Decimal;
  }>;
  finalResult: Decimal;
  calculatedAt: Date;
  calculatedBy: string; // user or system
  regulatoryBasis: string; // e.g., "FCA COBS 19.1"
}

function calculateFinalSalaryWithAudit(
  memberId: string,
  inputs: PensionInputs
): { result: Decimal; audit: CalculationAuditTrail } {
  const audit: CalculationAuditTrail = {
    memberId,
    calculationType: 'FINAL_SALARY',
    inputs: { ...inputs },
    steps: [],
    finalResult: new Decimal(0),
    calculatedAt: new Date(),
    calculatedBy: 'system',
    regulatoryBasis: 'Scheme Rules 2025 Section 4.2'
  };

  // Step 1: Calculate pensionable service
  const serviceYears = calculateServiceYears(inputs.joinDate, inputs.leaveDate);
  audit.steps.push({
    step: 'Calculate pensionable service',
    calculation: `days_between(${inputs.leaveDate}, ${inputs.joinDate}) / 365.25`,
    result: serviceYears
  });

  // Step 2: Apply accrual rate
  const accrued = inputs.finalSalary.times(inputs.accrualRate).times(serviceYears);
  audit.steps.push({
    step: 'Apply accrual rate',
    calculation: `${inputs.finalSalary} × ${inputs.accrualRate} × ${serviceYears}`,
    result: accrued
  });

  // Step 3: Round down to pence
  const finalAmount = accrued.toDecimalPlaces(2, Decimal.ROUND_DOWN);
  audit.steps.push({
    step: 'Round to pence (down)',
    calculation: `round_down(${accrued}, 2)`,
    result: finalAmount
  });

  audit.finalResult = finalAmount;

  // Persist audit trail
  savePensionCalculationAudit(audit);

  return { result: finalAmount, audit };
}
```

**Rules:**
- Log all calculation inputs
- Log each calculation step with formula
- Log intermediate results
- Log final result and rounding
- Store calculation timestamp and user
- Reference regulatory basis
- Make audit trail queryable for regulators

## Common Pension Calculation Patterns

### 1. Final Salary Pension

```typescript
/**
 * Calculate defined benefit pension based on final salary
 * Formula: Final Salary × Accrual Rate × Service Years
 */
function calculateFinalSalaryPension(
  finalSalary: Decimal,
  accrualRate: Decimal, // e.g., 1/60 or 1/80
  serviceYears: Decimal
): Decimal {
  return finalSalary
    .times(accrualRate)
    .times(serviceYears)
    .toDecimalPlaces(2, Decimal.ROUND_DOWN);
}

// Example: £50,000 salary, 1/60 accrual, 30 years service
// = £50,000 × (1/60) × 30 = £25,000 p.a.
```

### 2. CARE (Career Average) Pension

```typescript
/**
 * Calculate CARE pension with revaluation
 * Each year's pensionable earnings accrues benefit, revalued to retirement
 */
interface CAREYear {
  year: number;
  pensionableEarnings: Decimal;
  accrualRate: Decimal;
  revaluationFactor: Decimal; // CPI + X to retirement
}

function calculateCAREPension(careYears: CAREYear[]): Decimal {
  return careYears.reduce((total, year) => {
    const yearAccrual = year.pensionableEarnings
      .times(year.accrualRate)
      .times(year.revaluationFactor);

    return total.plus(yearAccrual);
  }, new Decimal(0)).toDecimalPlaces(2, Decimal.ROUND_DOWN);
}
```

### 3. Transfer Value Calculation

```typescript
/**
 * Calculate Cash Equivalent Transfer Value (CETV)
 * Heavily regulated - FCA COBS 19.1, TPR guidance
 */
interface TransferValueInputs {
  annualPension: Decimal;
  age: number;
  retirementAge: number;
  commutationFactor: Decimal; // From scheme actuary
  discountRate: Decimal; // UFPLS discount rate
  lifeExpectancy: number;
  inflation: Decimal;
  // ... many more factors
}

function calculateTransferValue(inputs: TransferValueInputs): {
  transferValue: Decimal;
  audit: CalculationAuditTrail;
  warnings: string[];
} {
  // CRITICAL: Transfer values are member's statutory right
  // Errors can cost £thousands and regulatory action

  const warnings: string[] = [];

  // Check if GMP applies (complex!)
  const gmpPortion = calculateGMPPortion(inputs);
  if (gmpPortion.greaterThan(0)) {
    warnings.push('Member has GMP - separate calculation required');
  }

  // Check if protected rights apply
  const protectedRights = checkProtectedRights(inputs);
  if (protectedRights) {
    warnings.push('Member has protected rights from contracting out');
  }

  // Main CETV calculation
  const cetv = inputs.annualPension
    .times(inputs.commutationFactor)
    .times(calculatePresentValue(inputs));

  // MUST be reviewed by actuary if > £30,000
  if (cetv.greaterThan(30000)) {
    warnings.push('CETV exceeds £30k - actuarial certification required');
  }

  return {
    transferValue: cetv.toDecimalPlaces(2, Decimal.ROUND_DOWN),
    audit: buildAuditTrail(inputs, cetv),
    warnings
  };
}
```

### 4. GMP (Guaranteed Minimum Pension)

```typescript
/**
 * GMP calculation for members who contracted out pre-1997
 * Extremely complex - different rates by gender, date ranges, revaluation rules
 */
interface GMPInputs {
  gender: 'M' | 'F';
  earningsHistory: Array<{
    taxYear: string; // e.g., '1990/91'
    earningsAboveLEL: Decimal; // Lower Earnings Limit
  }>;
  dateLeftService: Date;
  retirementDate: Date;
}

function calculateGMP(inputs: GMPInputs): Decimal {
  // WARNING: GMP calculations have different rules by period
  // Pre-1988, 1988-1997, post-1997
  // Different revaluation factors, different rates by gender
  // This is ACTUARIAL DOMAIN - always validate with actuary

  let totalGMP = new Decimal(0);

  for (const year of inputs.earningsHistory) {
    const accrualRate = getGMPAccrualRate(
      year.taxYear,
      inputs.gender
    );

    const yearGMP = year.earningsAboveLEL.times(accrualRate);
    totalGMP = totalGMP.plus(yearGMP);
  }

  // Revalue to retirement
  const revaluationFactor = getGMPRevaluationFactor(
    inputs.dateLeftService,
    inputs.retirementDate
  );

  return totalGMP
    .times(revaluationFactor)
    .toDecimalPlaces(2, Decimal.ROUND_DOWN);
}
```

## Anti-Patterns (DO NOT DO THIS)

### ❌ Anti-Pattern 1: Using Floating Point for Currency

```typescript
// WRONG - Will cause rounding errors
const pension = salary * 0.016666666 * 30; // 1/60 accrual
```

### ❌ Anti-Pattern 2: Hardcoded Rates/Factors

```typescript
// WRONG - Rates change, must be configurable
const commutationFactor = 20; // What year? What actuarial basis?
```

### ❌ Anti-Pattern 3: No Input Validation

```typescript
// WRONG - Silent failures, corrupt data
function calc(salary) {
  return salary * 0.02 * 30; // What if salary is null? Negative?
}
```

### ❌ Anti-Pattern 4: Black Box Calculations

```typescript
// WRONG - No audit trail, can't verify
const pension = complexPensionCalc(member);
// How was this calculated? What inputs? What version of rules?
```

### ❌ Anti-Pattern 5: Ignoring Edge Cases

```typescript
// WRONG - Doesn't handle:
// - Part-time service
// - Breaks in service
// - Early/late retirement factors
// - Death in service
// - Divorce orders
// - GMP integration
const pension = salary * rate * years; // Only works for simple case
```

## Testing Requirements

**Every pension calculation MUST have:**

### 1. Known Value Tests
```typescript
describe('Final Salary Calculation', () => {
  it('calculates correct pension for standard case', () => {
    // Known values from actuarial verification
    const result = calculateFinalSalaryPension(
      new Decimal(50000), // salary
      new Decimal(1).div(60), // 1/60 accrual
      new Decimal(30) // years
    );

    expect(result.toString()).toBe('25000.00');
  });
});
```

### 2. Edge Case Tests
```typescript
describe('Edge Cases', () => {
  it('handles zero service correctly', () => {
    const result = calculateFinalSalaryPension(
      new Decimal(50000),
      new Decimal(1).div(60),
      new Decimal(0)
    );
    expect(result.toString()).toBe('0.00');
  });

  it('handles fractional years correctly', () => {
    const result = calculateFinalSalaryPension(
      new Decimal(50000),
      new Decimal(1).div(60),
      new Decimal(15.5) // 15 years 6 months
    );
    expect(result.toString()).toBe('12916.66'); // Actuarial verified
  });

  it('throws on negative salary', () => {
    expect(() => {
      calculateFinalSalaryPension(
        new Decimal(-1000),
        new Decimal(1).div(60),
        new Decimal(10)
      );
    }).toThrow('Salary cannot be negative');
  });
});
```

### 3. Regulatory Compliance Tests
```typescript
describe('Regulatory Compliance', () => {
  it('produces audit trail for all calculations', () => {
    const { result, audit } = calculateFinalSalaryWithAudit(
      'MEM123',
      inputs
    );

    expect(audit.steps.length).toBeGreaterThan(0);
    expect(audit.regulatoryBasis).toBeDefined();
    expect(audit.calculatedAt).toBeInstanceOf(Date);
  });
});
```

### 4. Precision Tests
```typescript
describe('Precision', () => {
  it('rounds down to pence for liabilities', () => {
    const result = calculatePension(
      new Decimal(50000.999),
      new Decimal(1).div(60),
      new Decimal(1)
    );

    // Should round DOWN £833.34998... → £833.34
    expect(result.toString()).toBe('833.34');
  });
});
```

## Integration with Actuarial Systems

**Calculation results must be:**

1. **Comparable** with actuarial models (Excel, Prophet, etc.)
2. **Explainable** to actuaries (show your working)
3. **Verifiable** against regulatory returns
4. **Auditable** for scheme audits

**Always:**
- Document assumptions
- Reference scheme rules
- Note effective dates of rates/factors
- Flag calculations requiring actuarial sign-off

## Regulatory References

- **FCA COBS 19.1**: Transfer values and pension switching
- **TPR Code of Practice**: DB scheme funding
- **Pensions Act 2004**: Statutory requirements
- **Scheme Rules**: Specific to each pension scheme

## Resources

For deep dives on specific topics:
- [CARE Calculations](resources/care-calculations.md)
- [GMP Integration](resources/gmp-integration.md)
- [Transfer Values](resources/transfer-values.md)
- [Actuarial Factors](resources/actuarial-factors.md)
- [Testing Standards](resources/testing-standards.md)

## When This Skill Activates

**Automatically triggers when:**
- Keywords detected: pension, calculation, benefit, retirement, transfer
- Intent patterns: calculating/computing pension benefits
- File paths: `calculations/`, `services/pension/`
- Code patterns: `calculateBenefit`, `pensionAmount`, `transferValue`

**Enforcement:**
- BLOCKS proceeding without following standards
- Can override with `@skip-pension-standards` comment
- Should only skip if justified (document reason)

## Summary: The Rules

1. ✅ Use `Decimal.js` for all currency calculations
2. ✅ Round DOWN for liabilities (member benefits)
3. ✅ Validate ALL inputs explicitly
4. ✅ Create audit trail for every calculation
5. ✅ Use date libraries (NOT native Date)
6. ✅ Test against known actuarial values
7. ✅ Document regulatory basis
8. ✅ Flag calculations requiring actuarial sign-off
9. ✅ Handle edge cases (part-time, breaks, GMP)
10. ✅ Make calculations explainable to auditors

**If you're unsure, ask an actuary. Pension calculation errors can cost £millions.**

---

*This skill enforces 40+ years of pension industry lessons learned the hard way.*
