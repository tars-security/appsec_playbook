# Cloud Security Baseline Checklist

Provider-agnostic checklist — annotate with the specific service names for whichever provider(s)
are in use (AWS/GCP/Azure) when applying to a real environment. Organized by control area rather
than by provider console structure.

## Identity & access management

- [ ] Is there enforced MFA for all human console/CLI access, no exceptions for admin accounts?
- [ ] Is standing human access to production least-privilege, with elevated/admin access via
      just-in-time elevation (break-glass with approval + audit trail) rather than always-on?
- [ ] Are service-to-service and workload credentials short-lived/federated (workload identity,
      instance roles) rather than static long-lived keys?
- [ ] Is there periodic (quarterly minimum) access review removing unused grants, stale service
      accounts, and orphaned roles from departed employees or decommissioned services?
- [ ] Are IAM policy changes reviewed (PR-based IaC) rather than made ad hoc via console?
- [ ] Is root/organization-owner access tightly restricted and monitored separately from
      day-to-day admin access?

## Network

- [ ] Is there network segmentation between environments (prod/staging/dev) and between tiers
      (public-facing, app, data)?
- [ ] Are security groups/firewall rules default-deny, with explicit allow rules only — audited
      for overly broad rules (0.0.0.0/0 on anything beyond intended public endpoints)?
- [ ] Is the cloud metadata service protected against SSRF-based credential theft (IMDSv2-style
      enforcement or equivalent, network-level blocks from app processes that don't need it)?
- [ ] Are internal/admin services reachable only via VPN/private network/zero-trust proxy, never
      directly internet-exposed?

## Storage

- [ ] Are object storage buckets/containers default-private, with public access blocked at the
      account/org level unless explicitly and individually justified?
- [ ] Is encryption at rest enabled by default across storage, databases, and backups?
- [ ] Are backups access-controlled separately from primary data (a compromise of primary access
      shouldn't automatically grant backup access) and tested for restorability?
- [ ] Is there logging/alerting on storage permission changes, especially anything making a
      resource public?

## Secrets & key management

- [ ] Are application secrets stored in a managed secrets service, not environment variables baked
      into images or IaC state files?
- [ ] Is encryption key management centralized (KMS), with key access logged and least-privilege
      scoped?
- [ ] Is there a rotation policy for any secret that can't be made short-lived/dynamic?

## Logging, monitoring & detection

- [ ] Is cloud audit logging (control-plane API calls) enabled account/org-wide and shipped to a
      retained, tamper-resistant log store?
- [ ] Is there alerting on high-signal events: root/admin login, IAM policy changes granting broad
      access, security group changes opening ingress, disabling of logging itself?
- [ ] Is log retention sufficient for the org's incident response and compliance needs?

## Configuration & drift

- [ ] Is infrastructure primarily defined as code, with a low (and known) percentage of manual
      "click-ops" drift?
- [ ] Is there automated posture scanning (CSPM) catching misconfigurations continuously, not just
      at point-in-time audits?
- [ ] Is there a documented, prioritized process for acting on CSPM findings — otherwise it
      becomes a dashboard nobody reads?

## Multi-account / multi-project strategy

- [ ] Is there isolation between environments and between business units/products at the
      account/project level (blast radius containment), rather than everything in one flat
      account?
- [ ] Is there a landing-zone/guardrail baseline (org policies, SCPs, or equivalent) applied
      consistently to new accounts/projects as they're created, rather than each one starting from
      scratch?

## Cost as a security signal

- [ ] Is there billing anomaly alerting? Cryptomining and other resource-abuse incidents are
      frequently first detected via cost spikes, not security tooling — cheap signal worth wiring
      up early.
