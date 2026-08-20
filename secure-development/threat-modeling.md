# Threat Modeling Process

Lightweight, repeatable process meant to run in a 60–90 minute session per feature, not a
multi-day exercise. Optimized for actually happening consistently over theoretical completeness.

## When to trigger

Mandatory for:
- New auth/authz flows or changes to existing ones.
- Anything touching payments, PII/PCI/PHI, or regulated data.
- New external-facing API or integration (inbound or outbound).
- New admin/internal tooling with elevated data access.
- Significant architecture change (new trust boundary, new data store, new third-party
  dependency with data access).

Optional but encouraged: any feature the team itself flags as "this feels risky."

Not needed: internal UI-only changes, purely cosmetic features, changes with no new data flow or
trust boundary.

## Process (STRIDE-lite + data flow)

1. **Scope** (5 min): one paragraph — what's being built, what's explicitly out of scope for this
   review.
2. **Diagram the data flow** (15 min): actors, processes, data stores, trust boundaries. Whiteboard
   or diagram tool — doesn't need to be polished, needs to be accurate. This step alone surfaces
   most of the risk; don't skip it to jump to the checklist.
3. **Walk each trust boundary crossing** (20–30 min), asking per crossing:
   - **S**poofing — can the identity of either side be forged?
   - **T**ampering — can data in transit or at rest be modified undetected?
   - **R**epudiation — is there an audit trail for actions that need one?
   - **I**nformation disclosure — does this crossing expose more than the minimum needed?
   - **D**enial of service — can this be exhausted/abused to degrade the system?
   - **E**levation of privilege — can this crossing be used to gain access beyond what's intended?
4. **Prioritize findings** (10 min): score each with the
   [severity model](../vulnerability-management/severity-model.md) lens (likelihood ×
   impact, not just theoretical severity).
5. **Assign owners and ship decisions** (10 min): every finding gets one of: fix before ship, fix
   post-ship with ticket + deadline, or accept risk (see
   [risk acceptance template](../templates/risk-acceptance-template.md)). No finding leaves the
   session without a disposition.

Use [`templates/threat-model-template.md`](../templates/threat-model-template.md) to capture
output.

## Participants

- Required: feature's tech lead / primary engineer, AppSec facilitator.
- Strongly recommended: engineer who'll actually build the riskiest component.
- Optional: PM (useful for scope/impact context, not required for the technical walk).

## Facilitation notes

- Facilitate, don't lecture — the engineer should be doing most of the talking about how the
  system works; AppSec's job is asking the next question, not presenting a report.
- If the diagram reveals the engineer doesn't actually know how a piece works (e.g., "I assume the
  gateway checks that"), that assumption is itself a finding — verify it, don't take it on faith.
- Timebox strictly. A threat model that takes half a day won't happen a second time. If genuine
  complexity requires more time, split into a second session rather than letting the first one
  run long — protects the habit of doing this at all.
- Revisit if the design changes materially before ship — a threat model against a stale design is
  worse than none, because it creates false confidence.

## Anti-patterns

- Running STRIDE as a generic checklist disconnected from the actual data flow diagram — produces
  boilerplate findings nobody acts on.
- Treating threat modeling as a gate that blocks design docs from being approved, rather than a
  parallel input — creates incentive to route around it.
- Only threat modeling once a design is already fully built — too late to influence architecture,
  only useful for finding bugs at that point (which is what code review and testing are for).
