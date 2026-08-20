# Security Review — `<PR / feature / system name>`

For ad hoc security reviews (PR-level, design review, or vendor/tool review) that don't warrant a
full [threat model](threat-model-template.md).

## Metadata

- **Date:**
- **Reviewer:**
- **Requested by:**
- **Type:** `<PR review / design review / vendor-tool review / other>`
- **Link(s):** `<PR, design doc, vendor docs>`

## Context

`<What's being reviewed, and why it triggered a security review (risk trigger criteria met, or
requested ad hoc).>`

## Scope of this review

`<What was actually examined — be explicit about what wasn't reviewed, so the record doesn't imply
broader coverage than it had.>`

## Findings

| # | Finding | Severity | Recommendation | Disposition |
|---|---|---|---|---|
| 1 | | | | Fix before merge/ship / Track for later / Risk accepted / Informational only |

Severity per [`vulnerability-management/severity-model.md`](../vulnerability-management/severity-model.md).

## Outcome

- **Overall assessment:** `<approved / approved with follow-ups / blocked pending fixes>`
- **Follow-up tickets filed:** `<links>`
- **Risk acceptances filed (if any):** `<link to risk-acceptance-template.md instance>`

## Notes for future reviewers

`<Anything worth flagging for the next person who touches this system — a known limitation, a
deliberate tradeoff made here, a compensating control this review relied on.>`
