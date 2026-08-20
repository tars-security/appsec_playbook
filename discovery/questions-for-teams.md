# Discovery Interviews — Question Bank by Audience

Use these as conversation starters, not a script — the goal is calibrated trust and real signal,
not a survey. 30 minutes each. Note contradictions between what different roles believe to be true
— those gaps are findings in themselves.

## Engineering leads / staff engineers

- What's the system you'd least want a security review to look closely at right now, and why?
- Where does "we know it's not great but haven't had time" show up in this codebase?
- What's the last security-related incident (even a near-miss) on your team, and what changed
  afterward?
- If you had to bypass a security control to ship on time, which one, and how would you do it?
- What security tooling exists today that the team actively ignores or routes around?
- Who actually reviews high-risk PRs (auth, payments, data access) — is it consistent or
  person-dependent?

## Platform / infrastructure team

- What's provisioned by IaC vs clicked together manually, and roughly what percentage is which?
- Where do long-lived cloud credentials still exist, and who has them?
- What's the blast radius if this specific pipeline's credentials leaked — what could an attacker
  reach?
- Is there a change freeze / approval process for IAM policy changes, or is it self-service?
- What's the oldest unpatched/unsupported piece of infrastructure still load-bearing?

## Data / data platform team

- Where does PII actually live — including places it "shouldn't" (logs, analytics events, support
  tool exports, data warehouse copies)?
- Who has query access to the raw data warehouse, and is access reviewed periodically?
- Is there a data classification scheme, and is it enforced anywhere technically, or is it
  documentation only?
- What's the retention/deletion story for data subject to erasure requests (GDPR/CCPA if
  applicable)?

## Product / product security stakeholders

- What's the feature currently in flight that worries you most from a security angle?
- How early does security get looped into feature design today — at spec time, at PR time, or
  post-incident?
- What's a security ask from the past that got pushed back on, and why?

## IT / corporate security (if a separate function)

- Where's the line between AppSec and corp security responsibility, and is it actually respected
  in practice or does everything default to whoever answers first?
- What's the incident response process, and where does AppSec plug into it?
- Is there an existing vendor security review process, and does it cover software
  dependencies/SaaS tools engineering adopts unilaterally?

## Compliance / legal (if applicable)

- What frameworks are in scope today (SOC2, ISO 27001, PCI-DSS, HIPAA, etc.) and what's the audit
  cadence?
- What's the gap between "what the control says on paper" and "what actually happens" for the
  controls AppSec would own?
- Are there contractual security commitments to customers (pentest cadence, SLA on vuln
  remediation, breach notification terms) that aren't reflected in internal process yet?

## Leadership / your manager

- What does success look like in 90 days? In a year? Get this in their words, then compare to your
  own read from discovery — the gap is your first calibration conversation.
- What's the risk tolerance — is the org optimizing for velocity with acceptable risk, or is there
  a specific driver (upcoming audit, past incident, enterprise sales requirement) pushing toward
  stricter controls?
- What authority do you actually have — can you block a release, or is everything advisory until
  proven otherwise?
