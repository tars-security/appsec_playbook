# Risk Acceptance Record — `<short identifier>`

Formal record for a finding not being remediated (or not fully remediated) within its standard
SLA. Referenced from [`vulnerability-management/lifecycle.md`](../vulnerability-management/lifecycle.md).
No risk is accepted permanently by default — every record has an expiration and is reviewed at
that date, not silently renewed.

## Metadata

- **Date:**
- **Finding reference (ticket link):**
- **Severity:**
- **System/component affected:**
- **Requested by:**
- **Approved by:** `<name + role — must have actual authority for this severity level>`
- **Expiration date:** `<no permanent acceptances — set a concrete re-review date>`

## Finding summary

`<One paragraph: what the vulnerability is, and why it can't be remediated within standard SLA
(technical constraint, resourcing, business timeline, vendor dependency, etc.).>`

## Business justification

`<Why accepting this risk now is the right call — be specific about the tradeoff being made, not
just "we don't have time.">`

## Compensating controls (if any)

`<Anything reducing actual exploitability/impact in the meantime — e.g., network restriction,
monitoring/alerting added, feature flag limiting exposure. "None" is a valid answer, but state it
explicitly rather than leaving it blank.>`

## Residual risk assessment

`<Given compensating controls, what's the realistic remaining exposure? This is what the approver
is actually signing off on.>`

## Review plan

- **Re-review date:** `<matches expiration above>`
- **What happens at expiration:** `<remediate, renew with updated justification, or escalate if
  still unresolved — pick one as the default expectation, don't leave it open-ended>`

## Approval sign-off

Required approver level scales with severity — see
[`vulnerability-management/severity-model.md`](../vulnerability-management/severity-model.md) and
org policy (e.g., Medium → eng leadership, High/Critical → senior/executive leadership).

- **Approver name/role:**
- **Date approved:**
- **Signature/confirmation:** `<link to approval — email, ticket comment, or sign-off tool>`
