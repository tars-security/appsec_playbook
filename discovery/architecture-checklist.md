# Architecture Discovery Checklist

Run this against each major system/product line, not the org as a whole — architecture questions
need a concrete system in view to produce useful answers. Fill in `<placeholders>` per system.

## System inventory

- [ ] What are the deployment units (services, monoliths, functions — e.g. API A, auth-service,
      billing-monolith, image-resize-fn) and how many are there?
- [ ] Where does each run — cloud provider(s), regions, on-prem, hybrid?
- [ ] What's the source of truth for "what's running in prod right now" — is it accurate, or is
      there drift between IaC/config (Terraform, Pulumi, CloudFormation, Ansible, etc.) and
      reality?
- [ ] Is there a maintained service catalog / CMDB, or does this need to be built as a byproduct
      of this checklist? (e.g. Backstage, ServiceNow CMDB — tracks configuration items and their
      relationships, common in large enterprises but prone to drifting stale if not enforced by the
      deploy process — an inventory table in Notion/Confluence, IaC repo as source of truth, etc.)

## Trust boundaries and data flow

A trust boundary is any point where data moves between two parts of the system that don't trust each other by default — meaning that anything crossing that point must be checked (authenticated, validated, authorized) rather than trusted automatically.

Example — each arrow below crosses a boundary. Where a check happens, that's a boundary being
enforced correctly. Where no check happens, that's a gap:

```
  Browser  ───────►  API Gateway  ───────►  App Service  ───────►  Database
 (outside)  ✓ auth       (edge)    ✓ authz    (internal)   ✓ authz  (internal)
             checked                checked                checked

  3rd-party  ───────►  Webhook endpoint
  vendor      ✗ no signature check     ──── straight into the app, unverified
```

The first path checks identity and permissions at every hop, so the app can trust what reaches
the database. The webhook path skips verification entirely — that's the kind of gap this section
is meant to surface.

- [ ] Map the request path end to end: where it enters the system (ingress), which app services
      handle it, which databases it reaches, and where data eventually leaves (responses to users,
      calls to third parties).
- [ ] List every login system in use (e.g. admin panel, customer app, internal tools may each have
      their own). More separate auth systems means more places a mistake can happen.
- [ ] For each service, check whether it verifies permissions itself, or just trusts that an
      earlier component (like a gateway) already checked. A service that blindly trusts an
      upstream decision can be tricked into acting on a request it should have refused
      ("confused deputy" risk).
- [ ] Find connections that skip real authentication: internal services that trust each other just
      because they're on the same network, webhooks with no signature check, third-party callbacks
      accepted without verification. These are common ways an attacker who breaches one system
      moves into others.
- [ ] Identify where regulated data (PII/PCI/PHI) is stored, and trace every place it ends up —
      not just the main database, but also logs, backups, analytics pipelines, and the data
      warehouse.
- [ ] List third-party integrations that can write to production data, or read more of it than
      they strictly need. A breach on their side becomes a breach on yours.

## Identity and access

- [ ] What's the IdP, and is SSO enforced org-wide or are there gaps (legacy apps, vendor tools,
      infra consoles)?
- [ ] Is there a working model of "who can do what" for cloud/infra (IAM), or is it accretion of
      ad hoc grants?
- [ ] Are service-to-service credentials static secrets or short-lived/workload-identity based?
- [ ] Who has standing production access (SSH, DB console, admin panel), and is any of it
      break-glass-only vs always-on?

## Network and perimeter

- [ ] What's actually internet-facing? Cross-check the assumed list against an external scan —
      the gap between "what we think is exposed" and "what's exposed" is usually the first real
      finding.
- [ ] Is there a WAF/CDN in front of public endpoints, and is it in blocking or monitoring mode?
- [ ] Are internal networks flat or segmented? Can a compromised low-trust workload reach the data
      tier directly?
- [ ] Are admin interfaces (infra consoles, internal tools, CI/CD UI) internet-reachable or
      VPN/zero-trust gated?
- [ ] Does rate limiting exist anywhere in the request path today (CDN/gateway/app-level), or not
      at all? If it exists, is it global-only or per-user/per-key — global-only still leaves
      single-account abuse (credential stuffing, scraping, enumeration) unmitigated.

## Software supply chain

- [ ] What languages/frameworks dominate, and what's the oldest unmaintained one still in prod?
- [ ] Where do dependencies come from — public registries direct, or through an internal
      proxy/artifact repository?
- [ ] Is there any SBOM generation today, even partial?
- [ ] Who can publish to internal package registries, and is publishing gated by review?

## CI/CD and build

- [ ] What CI/CD platform(s), and is config centrally managed or per-repo free-for-all?
- [ ] What has write access to production deploy — humans, or exclusively pipelines?
- [ ] Are build artifacts signed/attested, or is "whatever the pipeline produced" trusted
      implicitly?
- [ ] Where do secrets used in CI live, and who can read them (this is frequently the highest-risk
      finding in a new environment)?

## Observability and detection

- [ ] Is there centralized logging for app + infra + auth events, and what's the retention?
- [ ] Would an anomalous auth event (impossible travel, mass data export, priv-esc) actually
      trigger an alert today, or only be visible in hindsight?
- [ ] Who owns detection/response if AppSec doesn't — is there a working relationship already, or
      does one need to be built?

## Output

Summarize per system as: architecture diagram (even rough), top 5 trust-boundary risks, and a
one-line maturity call (`ad hoc` / `developing` / `managed` / `optimized`) feeding into
[`maturity-assessment.md`](maturity-assessment.md).
