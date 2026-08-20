# Metrics & Reporting to Leadership

Principle: report outcome metrics (is risk actually decreasing) alongside output metrics (how much
work happened) — output-only metrics ("we scanned X repos, found Y findings") don't tell
leadership whether the org is safer, and invite the wrong optimization (finding volume over
risk reduction).

## Core metric set

### Vulnerability management
- MTTR by severity, trended over time (from [lifecycle.md](../vulnerability-management/lifecycle.md)).
- % findings remediated within SLA, by severity.
- Backlog size and age distribution (a shrinking backlog of old Lows while Criticals pile up is a
  worse trend than the aggregate number suggests — always break down by severity).
- Risk acceptance count and trend — rising risk acceptances is a signal worth surfacing on its
  own, not just remediation numbers.

### Coverage
- % of crown-jewel systems with SAST/SCA/secrets scanning active.
- % of high-risk PRs receiving security review.
- % of major features receiving threat modeling before ship.
- Maturity assessment score trend (from [`discovery/maturity-assessment.md`](../discovery/maturity-assessment.md)),
  re-scored quarterly.

### External validation
- Pentest finding severity trend release-over-release (are the same classes of bugs recurring?).
- Bug bounty program health metrics (see
  [`offensive-security/bug-bounty-playbook.md`](../offensive-security/bug-bounty-playbook.md)).

### Program health
- Security champions program engagement (findings raised by champions, features reviewed).
- False positive rate by scanning source (signals tooling health/tuning debt).

## Reporting cadence & audience

| Audience | Cadence | Focus |
|---|---|---|
| Engineering leadership (managers/directors) | Monthly | Team-level backlog/SLA status, upcoming asks, blockers needing their intervention |
| Executive leadership | Quarterly | Trend lines (not point-in-time snapshots), risk posture in business terms, roadmap and resourcing asks |
| Board / compliance (if applicable) | Per audit cycle or as contractually required | Framework-specific evidence, incident summary, material risk changes |
| Engineering org broadly | Quarterly or ad hoc | Wins, what changed for them (new tools/gates), how to get help |

## Presenting to executives

- Lead with trend, not snapshot — "MTTR for Critical findings dropped from 9 days to 3 days over
  two quarters" lands; "we have 4 open Critical findings" without context reads as either alarming
  or meaningless depending on the room.
- Translate technical severity into business terms once per report (what a Critical actually means
  for this org — customer data exposure, revenue system downtime, etc.) — don't assume the
  audience holds the technical severity model in their head.
- Always pair a risk number with what's being done about it and what's needed (budget, headcount,
  a specific decision from them) — a report that only informs, never asks, trains leadership to
  stop reading it closely.
- Show the "not doing yet" list deliberately — an honest scope statement builds more credibility
  than implying full coverage.

## Anti-patterns

- Vanity metrics with no decision attached (e.g., raw finding count with no severity/trend
  context).
- Metrics that incentivize the wrong behavior — e.g., rewarding "findings closed" without
  distinguishing genuinely fixed vs. quietly downgraded/risk-accepted to hit a number.
- Changing the metric definition without flagging it — silently redefining "coverage" makes trend
  lines meaningless and erodes trust when someone notices.
