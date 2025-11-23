# AI Lead Introduction Plan - Global Pension Consultancy
## Professional Rollout Strategy for Claude Code & Infrastructure

**Role**: AI Lead
**Start Date**: 2025-11-24
**Context**: Organization currently not using AI tooling
**Your Edge**: Production experience with Claude Code, pension domain knowledge, enterprise infrastructure patterns

---

## Executive Summary (Your Pitch)

You're introducing **controlled, auditable, enterprise-grade AI-assisted development** specifically configured for the pension industry's regulatory and accuracy requirements. This isn't about replacing developers - it's about:

1. **Consistency**: Enforcing pension calculation standards across all code
2. **Knowledge Capture**: Preserving domain expertise in reusable skills
3. **Velocity**: Accelerating delivery without sacrificing quality
4. **Risk Reduction**: Guardrails preventing common pension calculation errors
5. **Auditability**: Full tracking of AI interactions for compliance

---

## Week 1: Observation & Credibility Building
### Days 1-2: LISTEN FIRST

**Your Approach:**
- "I'm here to learn your processes first, then see where AI can help"
- Ask about their biggest development bottlenecks
- Identify pain points: repetitive calculations, documentation lag, knowledge silos
- Understand their tech stack (likely: .NET/C#, SQL, Excel integration, actuarial libraries)

**Key Questions to Ask:**
- "What takes the longest in your development cycle?"
- "Where do calculation errors typically come from?"
- "How do you ensure consistency across different developers?"
- "What happens when your actuarial expert goes on holiday?"
- "How much time is spent writing documentation vs. code?"

**What You're Really Doing:**
Mapping their pain to Claude Code's strengths - don't mention AI yet, just gather intelligence.

---

### Days 3-5: Small Demonstration (Build Trust)

**The "Accidentally Impressive" Demo:**

Pick ONE small, safe task that demonstrates value without threatening anyone:

**Option A: Documentation Generation**
- Take an existing pension calculation function
- Use Claude Code to generate comprehensive documentation
- Show before/after in 10 minutes
- Emphasize: "This is what I can do with AI assistance in minutes, not hours"

**Option B: Test Case Generation**
- Take a complex pension rule (e.g., CARE scheme calculation)
- Generate comprehensive test cases covering edge cases
- Show how AI understands pension domain logic
- Emphasize: "It caught 3 edge cases we hadn't documented"

**Option C: Code Review**
- Run the code-architecture-reviewer agent on existing codebase
- Generate professional architecture analysis
- Present findings diplomatically: "Here's what patterns the AI detected"
- Emphasize: "This could help with onboarding new developers"

**How to Present:**
- Casual, not a formal presentation
- "I was experimenting with something over lunch, thought you might find this interesting"
- Have it ready to show, don't make people wait
- Let them ask questions, don't oversell

---

## Week 2: Controlled Introduction
### The Professional Pitch (After You've Built Trust)

**Meeting Title**: "Controlled AI-Assisted Development for Pension Systems"

**Agenda** (30 minutes max):

1. **The Problem** (5 min)
   - Pension calculations are complex and high-risk
   - Knowledge silos when experts leave
   - Consistency challenges across developers
   - Documentation always lags code
   - Regulatory compliance requires auditability

2. **The Solution** (10 min)
   - Claude Code with enterprise infrastructure
   - Skills system: pension domain knowledge as code
   - Guardrails: prevent common calculation errors
   - Full audit trail of all AI interactions
   - Works within existing development workflow

3. **Live Demo** (10 min)
   - Show skill auto-activation on pension calculation
   - Show guardrail blocking an incorrect pattern
   - Show dev docs pattern for complex task tracking
   - Show agent generating test cases for pension logic

4. **Rollout Plan** (5 min)
   - Pilot with you only (Week 3-4)
   - Expand to volunteer developers (Week 5-6)
   - Measure: velocity, error rates, developer satisfaction
   - Decision point: continue, adjust, or pause

---

## Week 3-4: Pilot Implementation (You Only)

**Your Mission**: Build pension-specific infrastructure and prove ROI

### Build These Pension-Specific Skills:

**1. pension-calculation-standards**
- UK pension schemes (DB, DC, CARE)
- Calculation accuracy requirements (actuarial precision)
- Regulatory compliance patterns (FCA, TPR requirements)
- Common error patterns to avoid
- Test coverage standards for calculations

**2. actuarial-domain-knowledge**
- Terminology: commutation factors, transfer values, revaluation
- Standard formulas: pension input amounts, annual allowance
- Lifecycle events: retirement, death, divorce, transfers
- Edge cases: protected rights, GMP, contracting out

**3. pension-data-validation**
- Input validation for dates (scheme join, leaving, retirement)
- Salary validation and history
- Service credit calculations
- Age and eligibility checks
- Guardrails for impossible scenarios (e.g., retirement before birth)

**4. regulatory-compliance**
- GDPR considerations for pension data
- FCA compliance patterns
- Audit trail requirements
- Data retention rules
- Security standards for sensitive data

### Configure Auto-Activation Triggers:

**skill-rules.json for pensions:**
```json
{
  "pension-calculation-standards": {
    "type": "guardrail",
    "enforcement": "block",
    "promptTriggers": {
      "keywords": ["pension", "calculation", "benefit", "retirement"],
      "intentPatterns": ["calculate.*?(pension|benefit)", "compute.*?(retirement|transfer)"]
    },
    "fileTriggers": {
      "pathPatterns": ["**/calculations/**", "**/services/pension/**"],
      "contentPatterns": ["pensionAmount", "calculateBenefit", "commutationFactor"]
    }
  },
  "actuarial-domain-knowledge": {
    "type": "domain",
    "enforcement": "suggest",
    "promptTriggers": {
      "keywords": ["actuarial", "annuity", "commutation", "GMP", "revaluation"],
      "intentPatterns": ["(explain|understand).*?(actuarial|pension)"]
    }
  },
  "pension-data-validation": {
    "type": "guardrail",
    "enforcement": "warn",
    "fileTriggers": {
      "pathPatterns": ["**/validation/**", "**/dto/**"],
      "contentPatterns": ["validate", "schema", "zod"]
    }
  }
}
```

### Track Your Results:

**Metrics to Measure** (keep a spreadsheet):
- Time saved on documentation (before/after)
- Test coverage increase
- Calculation errors caught in review
- Developer onboarding time reduction
- Code consistency scores

**Concrete Examples to Capture**:
- "Generated 47 test cases for CARE scheme calculation in 12 minutes"
- "Caught potential GMP underpayment scenario in code review"
- "Documented entire transfer value calculation module in 20 minutes"
- "Skill auto-activated and prevented incorrect revaluation formula"

---

## Week 5-6: Expand to Volunteers

**Selection Criteria for Pilot Developers:**
- Open to new tools (not skeptics for first round)
- Working on non-critical path items
- Good communicators who'll give honest feedback
- Mix of senior and mid-level

**Onboarding Process** (per developer, 90 minutes):

1. **Context Setting** (15 min)
   - Why we're doing this
   - What success looks like
   - How to give feedback

2. **Installation & Setup** (30 min)
   - Install Claude Code
   - Copy the two essential hooks
   - Install pension-specific skills
   - Test skill activation

3. **Guided Exercise** (30 min)
   - Write a simple pension calculation together
   - Show skill activation in real-time
   - Demonstrate guardrail blocking incorrect pattern
   - Show dev docs pattern for task tracking

4. **Support & Check-ins** (15 min)
   - Daily Slack check-ins for first week
   - Weekly 30-min feedback sessions
   - Shared channel for questions

---

## Your Professional Talking Points

### When They Ask: "Is this safe for regulated industry?"

**Your Answer:**
"That's exactly why we're using this infrastructure approach rather than raw AI. We have:
- Guardrails that enforce pension calculation standards
- Full audit trail of every AI interaction
- Skills that embed our regulatory requirements
- Block enforcement for high-risk patterns
- Human review required for all calculations
- Version control of all AI-generated code

It's more controlled than a developer Googling Stack Overflow and copying code they don't fully understand."

### When They Ask: "Will this replace developers?"

**Your Answer:**
"No. It augments them. Pension calculations require domain expertise and judgment. This tool helps with:
- Boilerplate reduction
- Test case generation
- Documentation that doesn't lag
- Consistent application of our standards
- Faster onboarding of new developers

The actuarial and business logic still requires human expertise. This just removes the repetitive parts."

### When They Ask: "What about data security?"

**Your Answer:**
"Claude Code runs locally. Code doesn't leave our environment unless we explicitly send prompts to Claude's API, and we can configure what's allowed. We can:
- Restrict file access patterns
- Prevent sharing of sensitive data
- Run hooks that validate all interactions
- Maintain full audit logs
- Use local models if required (future option)

For pension data, we'll establish clear guidelines: no PII in prompts, anonymize examples, use synthetic test data."

### When They Ask: "How much does this cost?"

**Your Answer:**
"Claude Code itself is free. Claude API costs are usage-based. Based on my pilot:
- Average developer: £50-100/month in API costs
- Time saved: 5-10 hours/week on documentation and boilerplate
- ROI: Break-even at about 2 hours saved per month

We can start with a small pilot, measure actual costs, and decide if the ROI justifies expansion."

### When They Ask: "What if it makes mistakes in pension calculations?"

**Your Answer:**
"The same thing that happens if a developer makes mistakes: we catch them in code review, testing, and QA. The difference is:
- Our guardrail skills enforce pension calculation standards
- We generate more comprehensive test cases
- We have better documentation for reviewers
- We maintain consistency across developers

AI doesn't remove the need for review - it makes review more effective by catching common patterns and generating edge case tests."

---

## Risk Management (What Could Go Wrong)

### Risk 1: Skeptical Senior Developers

**Mitigation:**
- Involve them early as advisors, not guinea pigs
- Ask them to review your pension-specific skills
- Position as "help me get this right" not "I'm replacing you"
- Show, don't tell - demos beat arguments

### Risk 2: Calculation Error Blamed on AI

**Mitigation:**
- Document that ALL code (AI or human) goes through same review
- Maintain audit trail showing AI suggestions vs. final code
- Emphasize: AI is a tool, developer is responsible
- Have rollback plan ready

### Risk 3: Overpromising Early Results

**Mitigation:**
- Underpromise, overdeliver
- "This is an experiment" not "This will revolutionize everything"
- Share failures as learning, not hide them
- Measure conservatively

### Risk 4: Compliance/Legal Pushback

**Mitigation:**
- Brief legal/compliance BEFORE broad rollout
- Show audit trail capabilities
- Demonstrate guardrails and controls
- Offer to do compliance review of infrastructure
- Have TPR/FCA regulatory guidance ready

### Risk 5: Tool Becomes Crutch, Reduces Skill Development

**Mitigation:**
- Training program: how AI works, when to trust it
- Code review standards: reviewers must understand AI-generated code
- Skill development program continues
- AI augments learning, doesn't replace it

---

## Success Metrics (How You Prove Value)

### Quantitative (Track These Weekly):
- Time to complete standard tasks (before/after)
- Test coverage percentage (before/after)
- Documentation completeness (before/after)
- Code review cycle time (before/after)
- Developer satisfaction scores (survey)
- Calculation error rates (AI vs. manual)
- Onboarding time for new developers

### Qualitative (Capture These Stories):
- "AI caught an edge case we'd never considered"
- "Generated test data saved us 3 days of work"
- "New developer productive in week 1 instead of month 1"
- "Documentation finally matches the code"
- "Consistent patterns across 6 different services"

### ROI Calculation:
```
Cost per developer: ~£75/month (API usage)
Time saved per developer: ~8 hours/week
Hourly rate: ~£50/hour (pension developer)
Monthly value: 8hrs × 4 weeks × £50 = £1,600
Monthly ROI: (£1,600 - £75) / £75 = 2,033%

Even at 2 hours saved/week: £400 value - £75 cost = 433% ROI
```

---

## Your First Two Weeks: Day-by-Day Tactical Plan

### Week 1

**Monday (Day 1) - START**
- Morning: Introductions, setup, admin
- Afternoon: Meet team, understand current projects
- Evening: Review codebase, identify pain points

**Tuesday (Day 2)**
- Morning: Sit with developers, observe workflow
- Afternoon: Map pain points to Claude Code capabilities
- Evening: Prepare small demo

**Wednesday (Day 3)**
- Morning: Casual demo to 2-3 friendly developers
- Afternoon: Gather feedback, refine approach
- Evening: Draft formal pitch

**Thursday (Day 4)**
- Morning: Meet with technical leadership
- Afternoon: Formal pitch: "Controlled AI-Assisted Development"
- Evening: Adjust based on feedback

**Friday (Day 5)**
- Morning: If approved: begin pilot setup
- Afternoon: Install infrastructure, start building pension skills
- Evening: Document week 1 learnings

### Week 2

**Monday (Day 8)**
- Build pension-calculation-standards skill
- Configure skill-rules.json
- Test auto-activation

**Tuesday (Day 9)**
- Build actuarial-domain-knowledge skill
- Test on real pension calculation task
- Track time/quality metrics

**Wednesday (Day 10)**
- Build pension-data-validation skill
- Configure guardrails
- Test blocking behavior

**Thursday (Day 11)**
- Build regulatory-compliance skill
- Complete infrastructure setup
- Prepare volunteer developer onboarding

**Friday (Day 12)**
- Present Week 1-2 results
- Show concrete examples and metrics
- Propose Week 3-4 plan with volunteer developers
- Get approval for expansion

---

## Pension-Specific Use Cases (Your Demo Arsenal)

### Use Case 1: CARE Scheme Calculation
**Complexity**: Medium
**Risk**: High (calculation errors = underpayment/overpayment)
**AI Value**:
- Generate test cases for different career profiles
- Validate revaluation factor applications
- Document calculation steps for audit
- Check edge cases (part-time, breaks in service)

**Demo Script**:
1. Show pension calculation function
2. Ask Claude to generate test cases
3. Show it identifies edge cases: negative service, future dates, part-time adjustments
4. Show skill auto-activates with pension calculation standards

### Use Case 2: Transfer Value Calculation
**Complexity**: High
**Risk**: Very High (regulated, member gets cash)
**AI Value**:
- Validate calculation steps against regulations
- Generate comprehensive test scenarios
- Document assumptions and factors used
- Check for common errors (wrong discount rate, missing GMP)

**Demo Script**:
1. Show transfer value calculation code
2. Use agent to review for regulatory compliance
3. Show it catches missing GMP consideration
4. Show guardrail enforces actuarial review requirement

### Use Case 3: Annual Allowance Testing
**Complexity**: Very High
**Risk**: High (tax implications for members)
**AI Value**:
- Navigate complex regulations (tapered AA, MPAA)
- Generate test cases for different scenarios
- Document tax implications
- Validate input period logic

**Demo Script**:
1. Show annual allowance calculation
2. Ask Claude to explain the logic (tests understanding)
3. Generate test cases for edge scenarios
4. Show skill provides regulatory context

### Use Case 4: Data Migration Validation
**Complexity**: Medium
**Risk**: Very High (data quality = calculation quality)
**AI Value**:
- Generate validation rules from schema
- Create comprehensive test data
- Document transformation logic
- Check for data quality issues

**Demo Script**:
1. Show legacy data structure vs. new schema
2. Ask Claude to generate validation rules
3. Show it identifies orphan records, invalid dates, inconsistencies
4. Generate migration test suite

### Use Case 5: API Documentation for Pension Endpoints
**Complexity**: Low
**Risk**: Low (but high value)
**AI Value**:
- Generate OpenAPI specs from code
- Create example requests/responses
- Document pension-specific fields
- Maintain accuracy with code changes

**Demo Script**:
1. Show undocumented pension API endpoint
2. Generate comprehensive OpenAPI documentation
3. Show examples with realistic pension data
4. Update documentation after code change (shows maintenance)

---

## The "Oh Shit" Scenarios (And How to Handle Them)

### Scenario 1: AI Generates Incorrect Calculation

**What Happened**: Claude suggested a pension calculation with wrong formula
**Your Response**:
- "This is exactly why we have guardrails and code review"
- Show how guardrail caught it OR how review process would
- Document as learning: update skill to prevent recurrence
- Emphasize: same process as human error

**Follow-up Action**:
- Add anti-pattern to pension-calculation-standards skill
- Share learning with team
- Improve guardrail to catch this pattern

### Scenario 2: Developer Blindly Trusts AI Output

**What Happened**: Junior dev ships AI-generated code without understanding it
**Your Response**:
- "This is a training issue, not a tool issue"
- Implement mandatory code review requirement
- Create training: "How to Review AI-Generated Code"
- Update onboarding to emphasize developer responsibility

**Follow-up Action**:
- Add checkpoint: "Do you understand this code?"
- Pair programming for first month of AI usage
- Review standards updated

### Scenario 3: Compliance Raises Data Privacy Concerns

**What Happened**: Legal/compliance worried about pension data in AI prompts
**Your Response**:
- Show current data controls and audit trail
- Demonstrate no PII in prompts (use synthetic examples)
- Offer to add additional hooks for data validation
- Provide documentation: what data goes where

**Follow-up Action**:
- Create data classification guide for AI usage
- Add pre-prompt hook to scan for PII
- Document approved vs. prohibited data types
- Get compliance sign-off in writing

### Scenario 4: Senior Actuary Says "AI Doesn't Understand Pensions"

**What Happened**: Domain expert skeptical of AI capability
**Your Response**:
- "You're absolutely right - it doesn't. That's why we need your expertise in the skills"
- Invite them to review/improve pension-calculation-standards skill
- Show AI as tool that applies THEIR knowledge consistently
- Position them as author, not user

**Follow-up Action**:
- Make them skill advisor
- Regular reviews of AI suggestions with them
- Capture their expertise in skills
- Give them credit for improvements

### Scenario 5: Project Deadline Pressure, Team Wants to Skip Review

**What Happened**: "We trust the AI, let's ship it faster"
**Your Response**:
- "Absolutely not. Review is not optional."
- AI speeds up writing, not review
- Show risk: calculation error costs more than time saved
- Maintain standards or pause AI usage

**Follow-up Action**:
- Reinforce review requirements
- If pressure continues, escalate to leadership
- Consider if AI is creating false confidence
- Adjust training if needed

---

## Communication Templates

### Email 1: Week 1 Friday Update to Leadership

**Subject**: AI-Assisted Development - Week 1 Update

Hi [Leadership],

Quick update on my first week exploring AI-assisted development for our pension systems:

**This Week:**
- Observed current development workflows and pain points
- Conducted small demonstration of Claude Code capabilities
- Identified 5 high-value use cases for pension calculations
- Drafted controlled rollout plan with risk mitigations

**Key Findings:**
- Documentation lag is costing ~3 hours/week per developer
- Test case generation for pension calculations is manual and incomplete
- Knowledge silos when actuarial experts unavailable
- Consistency challenges across different developers

**Next Week:**
- Build pension-specific infrastructure (calculation standards, domain knowledge)
- Pilot on my own work to prove ROI
- Track metrics: time saved, quality improvements, error prevention

**Early Estimate:**
- Setup time: 2 days
- Projected ROI: 400-2000% based on time savings
- Risk level: Low (pilot with me only, all code reviewed)

Happy to discuss further.

[Your name]

---

### Email 2: Week 2 Friday - Pilot Results

**Subject**: AI Pilot Results - Request to Expand

Hi [Leadership],

Completed 2-week pilot of Claude Code with pension-specific infrastructure. Results below:

**Metrics:**
- Documentation time: 2 hours → 20 minutes (83% reduction)
- Test case generation: 4 hours → 45 minutes (81% reduction)
- Pension calculation review: Caught 3 edge cases not in original spec
- Code consistency: 100% adherence to standards (vs. ~70% manual)

**Concrete Examples:**
- CARE scheme calculation: Generated 47 test cases in 12 minutes
- Transfer value validation: Identified missing GMP consideration
- API documentation: Complete OpenAPI spec in 18 minutes
- Regulatory compliance: Auto-activated guardrails 8 times, prevented 2 errors

**Costs:**
- API usage: £42 for 2 weeks
- Time investment: 8 hours building infrastructure
- Time saved: ~16 hours in 2 weeks

**Proposal:**
Expand to 3-4 volunteer developers for 4-week pilot with:
- Structured onboarding (90 minutes each)
- Daily support and feedback
- Continued metric tracking
- Decision point in 4 weeks: continue, adjust, or pause

Risk remains low - all code reviewed, full audit trail, clear guidelines.

Recommend proceeding.

[Your name]

---

### Slack Message: Daily Developer Check-in

**Channel**: #ai-pilot
**Frequency**: Daily during volunteer phase

Morning team! Quick daily check-in:

**Yesterday's Wins:**
- Sarah: Generated comprehensive test suite for death benefits calculation
- James: Documented entire transfer service in 25 minutes
- Alex: Skill auto-activated and caught incorrect revaluation formula

**Blockers:**
- None currently

**Today's Focus:**
- Continue tracking time saved
- Any issues, ping me immediately
- Friday: weekly feedback session, 2pm

**Reminder**: All AI-generated code must be reviewed and understood before commit.

---

## Week 1-2 Checklist (Print This)

### Week 1: Observation & Trust Building
- [ ] Day 1: Meet team, understand current workflow
- [ ] Day 2: Map pain points to AI capabilities
- [ ] Day 3: Small casual demo to friendly developers
- [ ] Day 4: Formal pitch to technical leadership
- [ ] Day 5: Get approval, begin infrastructure setup
- [ ] Document learnings and feedback

### Week 2: Pilot Build
- [ ] Build pension-calculation-standards skill
- [ ] Build actuarial-domain-knowledge skill
- [ ] Build pension-data-validation skill
- [ ] Build regulatory-compliance skill
- [ ] Configure skill-rules.json with triggers
- [ ] Install essential hooks (skill-activation, post-tool-use)
- [ ] Test on real pension calculation task
- [ ] Track metrics: time, quality, errors caught
- [ ] Prepare results presentation
- [ ] Get approval for volunteer expansion

### Week 3-4: Volunteer Expansion (If Approved)
- [ ] Select 3-4 volunteer developers
- [ ] Schedule 90-minute onboarding sessions
- [ ] Create #ai-pilot Slack channel
- [ ] Daily check-ins
- [ ] Weekly feedback sessions
- [ ] Track expanded metrics
- [ ] Capture success stories
- [ ] Week 4: Present results and recommendations

---

## Confidence Builders (Read Before Day 1)

**You Know Pensions:**
- You've solved real pension problems with AI
- You understand the domain complexity
- You know where errors happen
- You've built working solutions

**You Know Claude Code:**
- You have production experience
- You understand the infrastructure patterns
- You know what works and what doesn't
- You've extracted best practices

**You Know How to Introduce Change:**
- Listen first, sell second
- Demonstrate value before asking for buy-in
- Start small, prove ROI, expand carefully
- Handle objections with evidence
- Manage risks proactively

**You're Not Claiming to Be Perfect:**
- This is an experiment, not a guaranteed solution
- You're learning alongside the team
- Failures are learning opportunities
- Adjust based on feedback

**Your Advantage:**
- You're the only one in the room with this experience
- You've done the hard work already (learning curve is behind you)
- You have a proven infrastructure to adapt
- You're coming in as the expert, act like it

---

## Final Advice

**First Two Weeks Are Critical:**
Build trust before asking for it. Show competence before claiming expertise. Listen more than you talk in Week 1, demonstrate more than you explain in Week 2.

**Don't Oversell:**
"Controlled AI-assisted development that could save 5-10 hours per developer per week" beats "AI will revolutionize our entire development process."

**Handle Skeptics with Demos, Not Arguments:**
"Let me show you" beats "Let me explain why you're wrong" every single time.

**Document Everything:**
Every win, every metric, every error caught, every hour saved. Your Week 3 pitch needs data, not promises.

**You've Got This:**
You've already done the hard part - learning how to use this stuff effectively. Now you're just teaching others and adapting it to a new domain you already understand.

Good luck tomorrow. You're going to be excellent.

---

## Appendix: Quick Reference

### Elevator Pitch (30 seconds)
"I'm introducing controlled AI-assisted development for our pension systems. It's not about replacing developers - it's about enforcing calculation standards, capturing actuarial expertise, and removing repetitive work. We have guardrails that prevent errors, full audit trails for compliance, and early pilots show 80% time savings on documentation and testing. Low risk, high ROI, measurable results."

### One-Liner When Someone Asks "What Do You Do?"
"I'm the AI Lead - I help our developers build pension systems faster and more consistently using controlled AI assistance."

### Response to "Sounds Risky"
"We thought so too. That's why we built guardrails, audit trails, and enforce human review. It's more controlled than Googling Stack Overflow."

### Response to "Will This Replace Me?"
"No. It removes the boring stuff so you can focus on the complex pension logic that actually requires expertise. Think of it as a very fast junior developer who does exactly what you tell it."

### Response to "I Don't Trust AI"
"Good. Neither do we. That's why everything goes through code review, we have blocking guardrails for risky patterns, and we maintain full audit trails. Trust but verify."

---

**Document Created**: 2025-11-23
**For**: AI Lead role at global pension consultancy
**Status**: Ready for Day 1

---

Good luck. You've got this.