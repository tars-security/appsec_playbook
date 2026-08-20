# AppSec Playbook

A working playbook for a Senior/Lead Application Security Engineer — the discovery process,
checklists, templates, and program-building patterns used when standing up or maturing an AppSec
function. Generic and portable by design: no company-specific names, stacks, or tools baked in,
so it can be dropped into any org and adapted. Written assuming AppSec fundamentals are known; no
101s.

## How this is organized

| Folder | Use it for |
|---|---|
| [`roadmap/`](roadmap/) | Generic 90-day sequencing plan: discovery, quick wins, program build-out |
| [`discovery/`](discovery/) | Structured way to learn any org's architecture, stack, and security maturity |
| [`secure-development/`](secure-development/) | Threat modeling, code review, API security, dependency/supply chain practices |
| [`vulnerability-management/`](vulnerability-management/) | Severity model and full lifecycle (detect → triage → remediate → verify) |
| [`ci-cd/`](ci-cd/) | Pipeline and GitHub Actions hardening, scanner integration |
| [`cloud/`](cloud/) | Cloud security baseline checklist (provider-agnostic, annotate per stack) |
| [`offensive-security/`](offensive-security/) | Bug bounty program ops, pentest coordination |
| [`governance/`](governance/) | Metrics/reporting to leadership, security champions program |
| [`templates/`](templates/) | Copy-paste artifacts: threat model, security review, risk acceptance, vuln report |

## Operating principles

- **Enable, don't gate.** Default to advisory + async review; reserve hard blocking (pipeline
  fail, release hold) for a short list of pre-agreed criteria (critical/high with known exploit
  path, secrets in code, auth bypass, etc.). Gate creep kills AppSec credibility faster than any
  single incident.
- **Risk-based, not checklist-based.** A finding without exploitability + business impact context
  is a data point, not a decision. Severity model lives in
  [`vulnerability-management/severity-model.md`](vulnerability-management/severity-model.md).
- **Every control needs an owner and a decay date.** Scanners drift out of tune, exceptions
  outlive their justification, champions rotate off teams. Revisit quarterly.
- **Instrument before you optimize.** Don't propose a new gate, tool, or process without a metric
  that will tell you if it worked. See [`governance/metrics-and-reporting.md`](governance/metrics-and-reporting.md).
- **Meet engineering where they are.** Findings ship as PR comments and tickets in the tools teams
  already use, in their language, with a suggested fix — not as a PDF from a scanner.

## How to use this

Every doc is written to be genericized-but-actionable: frameworks and checklists that need the
`<placeholders>` filled in for a specific org, not prose that needs a rewrite. Start with
[`roadmap/first-90-days.md`](roadmap/first-90-days.md) when entering a new org; otherwise pull
individual checklists/templates as needed.
