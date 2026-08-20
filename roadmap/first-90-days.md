# First 90 Days — Generic AppSec Entry Plan

Approach for entering an org. The *order* is what matters: understand before
you gate, gate before you scale.

## Phase 1 (Weeks 1–3) — Discovery, no gating

Goal: build an accurate mental model before touching anything that blocks engineering. Credibility
here is earned by asking pointed questions, not by deploying controls.

- [ ] Run a fast automated pass over the codebase and the live application using an AI-assisted
      offensive security tool (e.g., Shannon) to generate an initial risk signal ahead of manual
      discovery. Treat the results as hints, not facts — check anything important by hand before
      adding it to the top-10-risks list.
- [ ] Run [`discovery/architecture-checklist.md`](../discovery/architecture-checklist.md) against
      every production-facing system.
- [ ] Interview eng leads, platform/infra, data, and (if separate) IT/corp security using
      [`discovery/questions-for-teams.md`](../discovery/questions-for-teams.md).
- [ ] Meet with Product Lead(s) specifically, not just engineering — get visibility into the
      feature roadmap for the next 1–2 quarters so threat modeling starts proactively on what's
      coming rather than reactively on what already shipped.
- [ ] Inventory existing security tooling: SAST/DAST/SCA/secrets scanning, container/image
      scanning, WAF, IdP, vuln management platform, ticketing. Get read access to all of it before
      week 2 ends.
- [ ] Pull the last 12 months of: incidents/postmortems, pentest reports, bug bounty history,
      audit findings (SOC2/ISO/PCI if applicable). This is the fastest way to learn where the
      bodies are buried.
- [ ] Run [`discovery/maturity-assessment.md`](../discovery/maturity-assessment.md) and produce a
      baseline score — this becomes the diff you show progress against later.
- [ ] Identify the 3–5 crown-jewel systems (auth, payments, PII stores, admin/internal tooling
      with broad access) — everything downstream prioritizes around these.

Deliverable: a short internal doc (not a slide deck) — architecture summary, tooling inventory,
top 10 risks observed, crown jewels list. Share with your manager and eng leadership for
correction, not approval — you're calibrating, not presenting.

## Phase 2 (Weeks 4–6) — Quick wins + trust building

Goal: fix visible, uncontroversial things fast. Avoid anything requiring a cross-team process
change yet — you don't have the trust or context to spend that capital well.

- [ ] Close any critical findings sitting untriaged in existing scanner backlogs.
- [ ] Fix or flag anything from Phase 1 that's a "everyone will agree this is bad" issue:
      exposed secrets, public buckets/repos, unpatched critical CVEs on internet-facing assets,
      default credentials.
- [ ] If a bug bounty or pentest program exists, review the backlog personally — don't inherit
      someone else's triage without re-checking it.
- [ ] Establish your communication channel(s): a Slack/Teams channel for AppSec questions, and a
      lightweight intake process (ticket template) for "is this safe" questions from engineering.
- [ ] Start 1:1s with the engineers who'll be your actual day-to-day collaborators — not just
      leads. They'll tell you what leads won't.
- [ ] Draft (don't publish yet) the severity model —
      [`vulnerability-management/severity-model.md`](../vulnerability-management/severity-model.md)
      — and sanity-check it against 5–10 real historical findings.
- [ ] Run the [`ci-cd/scanner-depth-audit.md`](../ci-cd/scanner-depth-audit.md) checklist against
      whatever SAST/DAST/fuzzing tooling already exists — determines whether current scanner
      signal can be trusted for the Phase 3 advisory rollout, or needs retuning first.

Deliverable: visible fixes shipped, a working intake channel, and a severity model draft
circulated for feedback.

## Phase 3 (Weeks 7–10) — Program foundations

Goal: put the durable pieces in place. This is where you start spending trust capital on process
change — spend it on the highest-leverage items only.

- [ ] Publish the severity model + SLA/lifecycle
      ([`vulnerability-management/lifecycle.md`](../vulnerability-management/lifecycle.md)) —
      this is the contract that makes everything downstream (gating, reporting, escalation)
      legible and fair.
- [ ] Get SAST/SCA/secrets scanning running in CI for the crown-jewel repos first, advisory mode
      only (no blocking yet). See [`ci-cd/pipeline-security.md`](../ci-cd/pipeline-security.md).
- [ ] Ship the threat modeling process
      ([`secure-development/threat-modeling.md`](../secure-development/threat-modeling.md)) and
      run it live on one upcoming major feature — pick one with an engaged, security-friendly team
      lead for the first run.
- [ ] Stand up (or formalize) the code review checklist
      ([`secure-development/code-review-checklist.md`](../secure-development/code-review-checklist.md))
      for high-risk PRs (auth, payments, data access, deserialization, file upload).
- [ ] If none exists, scope a pentest for the crown-jewel systems using
      [`offensive-security/pentest-coordination.md`](../offensive-security/pentest-coordination.md).
- [ ] Define the escalation path for a live incident (who's paged, who has authority to hold a
      release) — codify with security incident response / IR owners, don't own IR yourself unless
      that's explicitly in scope.

Deliverable: published severity model + SLAs, advisory scanning live on crown jewels, first threat
model completed, pentest scoped or scheduled.

## Phase 4 (Weeks 11–13) — Scale and report

Goal: move from "I did things" to "the org has a program." Show leadership the trajectory, not
just a point-in-time snapshot.

- [ ] Expand scanning coverage beyond crown jewels; decide gating criteria and a rollout plan for
      any hard blocks (with an exception process defined before the first block happens, not
      after).
- [ ] Recruit and launch a security champions cohort if the org is large enough to need
      distributed coverage — see
      [`governance/security-champions.md`](../governance/security-champions.md).
- [ ] Build the first metrics dashboard/report —
      [`governance/metrics-and-reporting.md`](../governance/metrics-and-reporting.md) — covering
      MTTR, backlog age/severity mix, and coverage %.
- [ ] Re-run the maturity assessment; present the 90-day diff to leadership alongside a 6–12 month
      roadmap prioritized by risk reduction per unit of engineering friction imposed.
- [ ] Decide what you're explicitly *not* doing yet, and say so out loud — an honest "not this
      quarter" list builds more trust than a silently growing backlog of unstated gaps.

Deliverable: 90-day retro (baseline → now), published roadmap for the next 2–3 quarters, first
recurring metrics report cadence agreed with leadership.

## Anti-patterns to avoid in this window

- Shipping a blocking gate before the severity model and exception process both exist and are
  socialized — this is the single fastest way to get shadow-IT'd around.
- Running a threat model or review checklist that reads like a generic OWASP list with no
  org-specific context — engineers disengage immediately.
- Presenting findings without proposed fixes or effort estimates — "here's 200 vulnerabilities"
  is not a plan.
- Trying to own incident response, compliance, and physical/corp security simultaneously with
  AppSec unless explicitly scoped that way — pick the lane and be excellent in it first.
