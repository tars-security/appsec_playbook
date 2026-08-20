# GitHub Actions Security Checklist

GitHub-Actions-specific hardening, complementing the general principles in
[`pipeline-security.md`](pipeline-security.md). Applicable directly if GitHub Actions is the
CI/CD platform; if not, use as a reference for the equivalent controls on whatever platform is in
use (GitLab CI, CircleCI, Jenkins, etc. all have analogous risks).

## Workflow permissions

- [ ] Is the default `GITHUB_TOKEN` permission set to read-only at the org/repo level, with
      elevated permissions (`contents: write`, etc.) granted explicitly per-job only where needed?
- [ ] Are `pull_request_target` and `workflow_run` triggers avoided or carefully guarded — these
      run with the base repo's permissions/secrets even when triggered by a fork PR, a common
      source of secret-exfiltration vulnerabilities?
- [ ] If `pull_request_target` is used (e.g., for label automation), does it avoid checking out
      and executing untrusted PR head content?

## Third-party actions

- [ ] Are all third-party actions pinned to a full commit SHA, not a tag or branch reference?
- [ ] Is there an org-level allowlist restricting which actions/publishers can be used
      (`actions: allowed-actions` policy at the org level)?
- [ ] Are actions from unverified/low-reputation publishers avoided for anything touching secrets
      or deploy steps?
- [ ] Is Dependabot (or equivalent) monitoring action dependencies for known-vulnerable versions?

## Secrets

- [ ] Are repo/org secrets scoped to only the environments/workflows that need them (GitHub
      Environments with required reviewers for production secrets)?
- [ ] Is OIDC federation used for cloud deploy credentials instead of long-lived static secrets
      stored as GitHub secrets?
- [ ] Are secrets excluded from being passed to untrusted code paths (e.g., not exposed to a step
      that runs on a fork PR)?

## Self-hosted runners (if used)

- [ ] Are self-hosted runners never used on public repos with `pull_request` triggers from forks
      (a fork PR can execute arbitrary code on the runner, and self-hosted runners typically have
      broader network/credential access than GitHub-hosted ones)?
- [ ] Are self-hosted runners ephemeral (fresh VM/container per job) rather than persistent,
      limiting cross-job contamination and credential persistence?
- [ ] Is runner network access scoped (not flat access to the full internal network)?

## Branch protection & merge controls

- [ ] Is the default branch protected: required reviews, required status checks (including
      security scans), no direct pushes?
- [ ] Are workflow file changes (`.github/workflows/`) subject to the same review requirement as
      any other code change — specifically checked, since this is a common bypass path?
- [ ] Is force-push to protected branches disabled?
- [ ] Are required status checks actually required for admins too, or can admins bypass (verify
      this explicitly — it's a common gap)?

## Artifact & release integrity

- [ ] Are release artifacts built from tagged, reviewed commits only — not from arbitrary
      workflow dispatch with unreviewed input?
- [ ] Is artifact attestation (`actions/attest-build-provenance` or equivalent) used for anything
      distributed externally?

## Monitoring

- [ ] Is there audit log monitoring for suspicious Actions activity (new workflow added by a
      first-time contributor, secrets access from an unusual workflow, runner registration
      events)?
- [ ] Is there alerting if org-level Actions policy (allowed actions, runner group access) is
      changed?
