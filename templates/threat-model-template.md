# Threat Model — `<feature/system name>`

Companion to [`secure-development/threat-modeling.md`](../secure-development/threat-modeling.md).
Copy this file per review; keep it short enough to actually get filled in during the session.

## Metadata

- **Date:**
- **Feature/system:**
- **Participants:**
- **Facilitator:**
- **Design doc / spec link:**

## Scope

`<One paragraph: what's being built, what's explicitly out of scope for this review.>`

## Data flow diagram

`<Link to diagram, or embed a simple description: actors → processes → data stores, with trust
boundaries marked.>`

## Trust boundary walkthrough

| # | Boundary crossing | S | T | R | I | D | E | Notes |
|---|---|---|---|---|---|---|---|---|
| 1 | `<e.g., client → API gateway>` | | | | | | | |
| 2 | `<e.g., service → third-party webhook>` | | | | | | | |

Mark each STRIDE column ✓ (considered, no issue) / ⚠ (finding raised, see below) / — (not
applicable to this crossing).

## Findings

| # | Finding | Severity | Disposition | Owner | Due date |
|---|---|---|---|---|---|
| 1 | | | Fix before ship / Fix post-ship / Risk accepted | | |

Severity per [`vulnerability-management/severity-model.md`](../vulnerability-management/severity-model.md).
Risk-accepted findings need a linked
[risk acceptance record](risk-acceptance-template.md).

## Assumptions verified during session

`<Anything that was assumed true but not actually verified before this session — e.g., "assumed
the gateway enforces auth" — record whether it was confirmed or turned out to be a finding.>`

## Follow-up

- [ ] All findings entered into vulnerability tracker with owners/SLAs.
- [ ] Re-review scheduled if design changes materially before ship.
