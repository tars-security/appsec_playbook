# Bug Bounty Program Playbook

Covers running a private (invite-only) or public program end-to-end. Assumes a bounty platform
(HackerOne, Bugcrowd, Yogosha, Intigriti, etc.) or a direct researcher relationship — adapt
platform-specific mechanics as needed.

## Before launch

- [ ] Define scope precisely: in-scope assets (with explicit environment — prod only, unless
      staging is intentionally included), out-of-scope assets, and explicitly out-of-scope
      vulnerability classes (e.g., self-XSS, missing security headers without demonstrated impact,
      social engineering, physical) to reduce low-value noise.
- [ ] Set the reward table by severity, calibrated to the
      [severity model](../vulnerability-management/severity-model.md) — publish it; ambiguity
      here is the top source of researcher frustration and program churn.
- [ ] Confirm the org can actually triage and fix findings at the pace a program will generate
      them before opening — a program with a growing untouched backlog damages researcher trust
      and, if public, the org's public reputation.
- [ ] Decide private vs public launch. Private (curated researcher list) first is almost always
      right for a new program — validates triage capacity and reward calibration before opening to
      volume.
- [ ] Set up the intake → triage → remediation pipeline feeding into the standard
      [vulnerability lifecycle](../vulnerability-management/lifecycle.md), with bounty-specific
      SLA commitments (first response time, time to triage) published to researchers.
- [ ] Establish a legal safe-harbor statement so researchers acting in good faith within scope are
      protected from legal action.

## Triage

- [ ] First response SLA (target: <24–48h) even if just acknowledging receipt — researcher
      experience is largely determined by responsiveness, independent of final payout.
- [ ] Validate reproduction before engaging on severity/reward — ask for a clear PoC/repro steps
      if missing rather than guessing.
- [ ] Score severity independently using the org's model — don't default to the researcher's
      self-assessed severity, but explain the reasoning if it differs, respectfully.
- [ ] Deduplicate against known issues (including internally known-but-unfixed issues) —
      communicate duplicate status promptly and honestly.
- [ ] Route to the owning team through the same vulnerability lifecycle as any other finding —
      bug bounty findings don't get a separate remediation track.

## Researcher relationship management

- [ ] Communicate proactively on remediation timelines, even when the news is "this will take
      longer than usual" — silence is what drives researchers to escalate publicly or lose
      engagement with the program.
- [ ] Pay promptly once severity/reward is agreed — payment delays are one of the most common
      public complaints about programs and directly affect researcher engagement quality.
- [ ] Consider a recognition mechanism beyond pure bounty (hall of fame, swag, bonus for
      exceptional reports) to build a relationship with high-quality researchers specifically —
      a small number of researchers typically produce a large share of the high-value findings.
- [ ] Handle disputes (severity disagreement, duplicate disputes) with a clear escalation path and
      willingness to reconsider with new evidence, not a rigid final-word stance.

## Disclosure

- [ ] Define the coordinated disclosure timeline (e.g., 90 days from fix, or per-report
      negotiation) and honor it consistently.
- [ ] Decide policy on public disclosure of fixed findings (some programs publish a redacted
      changelog/hall of fame; others keep everything private) — be explicit about which, so
      researcher expectations are set correctly at program launch.

## Program health metrics

Track and review periodically (feed into
[`governance/metrics-and-reporting.md`](../governance/metrics-and-reporting.md)):
- Mean time to first response / to triage / to resolution.
- Valid finding rate (signals scope clarity and researcher quality/fit).
- Severity distribution trend over time (a healthy maturing program should trend toward fewer
  High/Critical over time as the same researcher pool re-tests a hardening surface).
- Cost per valid finding, and whether it's trending toward or away from cost-effectiveness vs.
  scheduled pentests for the same surface.
- Researcher retention/repeat-participation rate.

## Scaling scope

- [ ] Expand scope incrementally as triage capacity is proven, not all-at-once.
- [ ] Re-evaluate reward table periodically against market rate (platform benchmarks) — an
      under-rewarded program bleeds researcher attention to better-paying programs over time.
