# Security Champions Program

Distributed-ownership model for orgs where central AppSec can't be in every design review or PR.
Worth standing up once the org is large enough that AppSec can no longer maintain direct
relationships with every team (rough signal: more than ~5–8 engineering teams).

## Program design

- [ ] One champion per team (not per individual — the role attaches to the team, person can
      rotate), ideally someone already engaged/curious about security rather than voluntold.
- [ ] Define the role explicitly: first point of contact for security questions on their team,
      participates in threat modeling for their team's features, raises findings from their own
      code review, attends a recurring champions sync.
- [ ] Time allocation should be explicit and small (e.g., ~10–15% of time) and agreed with their
      manager — an unstated/unbounded expectation is how champions programs quietly die from
      under-resourcing.
- [ ] Provide a clear escalation path back to AppSec for anything beyond the champion's
      confidence/authority — champions are a force multiplier, not a replacement for AppSec
      expertise on hard calls.

## Enablement

- [ ] Structured onboarding: walk through the [threat modeling process](../secure-development/threat-modeling.md),
      the [code review checklist](../secure-development/code-review-checklist.md), and the
      [vulnerability lifecycle](../vulnerability-management/lifecycle.md) — champions should be
      fluent in the org's actual process, not generic security theory.
- [ ] Recurring sync (biweekly or monthly): recent findings/incidents worth broad awareness, new
      tooling/process changes, open Q&A.
- [ ] Direct line to AppSec (dedicated Slack channel or equivalent) for fast answers — champions
      lose engagement fast if their questions sit unanswered.
- [ ] Consider light hands-on training (a CTF-style exercise, a guided vuln-finding session in a
      real or intentionally vulnerable app) over pure lecture content — retention is much higher.

## Recognition & sustainability

- [ ] Make champion contributions visible to their management chain — a champion who finds and
      helps fix a real issue should get credit that shows up in their performance narrative, not
      just a thank-you in a Slack thread.
- [ ] Rotate the role periodically (e.g., annually) rather than assuming permanence — prevents
      burnout and spreads security fluency wider across the org over time.
- [ ] Track program engagement as a metric (see
      [`governance/metrics-and-reporting.md`](metrics-and-reporting.md)) — a champions program with
      no measurable output after a couple of quarters needs a structural fix, not just more
      cheerleading.

## Common failure modes

- Champions selected by management assignment with no interest in the role — engagement dies
  quickly; prefer volunteers even if it means uneven initial coverage.
- No real authority or visible impact — if champions raise findings that go nowhere, the role
  becomes a checkbox instead of meaningful ownership.
- AppSec treating champions as free triage labor rather than investing in their growth — the
  program should feel like a career/skill benefit to participants, not an unpaid tax.
- Program launched but never revisited — roles go stale, coverage gaps open as teams reorg, and
  nobody notices until an incident reveals a team with no current champion.
