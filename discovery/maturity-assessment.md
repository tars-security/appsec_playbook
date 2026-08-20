# AppSec Maturity Assessment

A lightweight scoring model to baseline an org and re-measure over time. Not a substitute for
BSIMM/OWASP SAMM if the org wants a formal benchmark — this is a fast, opinionated version tuned
for a first-90-days baseline and quarterly re-checks.

## Scoring

Score each dimension 0–3:

- **0 — Absent**: no process, ad hoc or accidental only.
- **1 — Ad hoc**: exists inconsistently, person-dependent, undocumented.
- **2 — Developing**: documented and applied to some/most of the estate, gaps known.
- **3 — Managed**: consistently applied, measured, owned, and reviewed on a cadence.

(A 4 — "Optimized" — tier exists in maturity models generally, but isn't a realistic first-90-days
target; don't score for it yet.)

## Dimensions

| Dimension | 0 | 1 | 2 | 3 | Score | Notes |
|---|---|---|---|---|---|---|
| Architecture visibility | No system inventory | Partial, stale | Documented, mostly current | Living, auto-updated | | |
| Threat modeling | Never done | Done reactively post-incident | Done for some new major features | Standard part of design for all major features | | |
| Secure code review | No security-specific review | Informal, ad hoc reviewer | Checklist exists, used inconsistently | Enforced for high-risk changes, tracked | | |
| SAST/SCA in CI | None | Exists, not enforced, high noise | Tuned, advisory, decent signal | Enforced with agreed gating criteria | | |
| Secrets management | Secrets in code/config routinely | Scanning exists, reactive cleanup | Scanning + prevention (pre-commit/pipeline block) | Centralized secrets manager, short-lived creds standard | | |
| Dependency/SBOM | No visibility into dependencies | Manual, occasional audit | Automated SCA, no SBOM | SBOM generated + tracked per release | | |
| Vulnerability management | No central tracking | Tracked per-team, inconsistent SLAs | Central tracking, defined severity model | Enforced SLAs, measured MTTR, escalation path | | |
| Pentest / external validation | Never tested | Ad hoc, no follow-through on findings | Regular cadence, findings tracked | Regular cadence + retest + trend tracked over time | | |
| Bug bounty / responsible disclosure | No channel to report | security.txt or inbox, no SLA | Private program, triage process exists | Program with measured MTTR, researcher relationship managed | | |
| CI/CD pipeline security | Anyone can modify pipeline/deploy | Some access control, no audit trail | Reviewed changes, audit trail exists | Signed artifacts, least-privilege deploy, full audit trail | | |
| Container/image security | No image scanning, floating/unpinned base images | Scanning exists, not enforced or registry-only | Scanning pre-push, enforced CVE threshold | Enforced threshold + base image staleness tracked + minimal/distroless images standard | | |
| Cloud/IAM hygiene | Broad standing access, no review | Some least-privilege, no periodic review | Least-privilege mostly enforced, periodic access review | Least-privilege + automated drift detection + JIT access | | |
| Incident response | No defined process | Process exists, untested | Tested via tabletop, AppSec integrated | Regularly tested, post-incident action items tracked to closure | | |
| Security champions / culture | No distributed ownership | Informal go-to people per team | Formal champions program, inconsistent engagement | Champions program with measurable output (findings raised, features reviewed) | | |
| Metrics & reporting to leadership | None | Ad hoc, reactive only | Regular report, mostly output metrics (counts) | Regular report with outcome metrics (risk trend, MTTR, coverage) | | |

## Using the score

- Total possible: 42. Don't chase a perfect score — chase the dimensions with the highest
  risk-reduction-per-effort given the crown jewels identified in
  [`architecture-checklist.md`](architecture-checklist.md).
- Re-score quarterly. The delta, not the absolute number, is what you report to leadership — see
  [`governance/metrics-and-reporting.md`](../governance/metrics-and-reporting.md).
- A dimension stuck at the same score for two consecutive quarters despite stated effort is a
  signal to change approach, not just keep pushing the same lever.
