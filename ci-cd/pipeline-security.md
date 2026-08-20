# CI/CD Pipeline Security

Covers the pipeline as an attack surface in its own right (it typically holds the highest-value
credentials in the org — deploy access, cloud IAM, signing keys) as well as its role running
security scanning.

## Pipeline as attack surface

- [ ] Who can modify pipeline definitions (`.yml`/config files)? If it's gated the same as any
      other code change (PR + review), good; if pipeline config changes bypass review, that's a
      direct path to credential exfiltration or supply-chain compromise.
- [ ] Are pipeline runs triggered by PRs from forks restricted from accessing secrets (prevents a
      malicious PR from exfiltrating deploy credentials via a modified workflow file)?
- [ ] Are third-party actions/plugins pinned to a commit SHA, not a mutable tag (`@v1` can be
      repointed by the action's maintainer or a compromised account)?
- [ ] Is there an allowlist/review process for new third-party actions/plugins, or can any
      contributor add one unilaterally?
- [ ] Are pipeline logs scrubbed of secrets (many leaks happen via `echo`/debug output rather than
      the secret store itself)?

## Credentials & secrets in CI

- [ ] Are pipeline credentials scoped per-job/per-repo (least privilege), not one shared
      god-credential across all pipelines?
- [ ] Are deploy credentials short-lived (OIDC federation to cloud provider) rather than static
      long-lived keys stored as CI secrets?
- [ ] Is there a secrets scanner running pre-commit and in-pipeline, catching both new secrets and
      historical ones already in the repo?
- [ ] Is there a rotation process (and forcing function) for any secret that must remain static?

## Build integrity

- [ ] Are build artifacts reproducible/verifiable, or does "trust whatever came out of the
      pipeline" go unchallenged?
- [ ] Are artifacts signed at build time, with signature verification enforced before deploy?
- [ ] Is the deploy step separated from the build step with an approval gate for production
      (even if lightweight), so a compromised build step can't unilaterally deploy?

## Scanning integration (SAST/DAST/SCA/secrets/container)

Covers whether scanning is wired in correctly. For whether the SAST/DAST/fuzzing signal itself is
actually trustworthy (rule quality, coverage, false positive rate), see
[`scanner-depth-audit.md`](scanner-depth-audit.md).

- [ ] Scanning stage placement: PR-time (fast feedback, advisory) vs pre-merge gate vs
      pre-deploy gate — match strictness to how confident you are in signal quality. Don't gate on
      a scanner you haven't tuned yet.
- [ ] Is there a defined noise budget — if a scanner's false positive rate stays above a threshold
      for N weeks, is there an owner responsible for tuning it, or does it just get ignored by
      engineering?
- [ ] Do findings route to where engineers already work (PR comment, ticket in their tracker) with
      enough context to act, rather than requiring a login to a separate security dashboard?
- [ ] Is there a clear, fast path for a team to flag a false positive and have it suppressed
      (with reasoning recorded) without waiting on AppSec as a bottleneck?

## Gating policy

- [ ] Is the list of what blocks a merge/deploy explicit and published (not tribal knowledge)?
- [ ] Is there an exception/override path for a legitimate urgent deploy, with an audit trail of
      who overrode and why?
- [ ] Is gating criteria reviewed periodically as scanner tuning improves — a policy set at
      rollout with noisy scanners will be miscalibrated once they're tuned, in either direction?

## Container/image security (if applicable)

- [ ] Is base image provenance controlled (approved base images from an internal registry, not
      arbitrary public images)?
- [ ] Is image scanning run pre-push and pre-deploy, with a policy on CVE severity thresholds?
- [ ] Are images scanned for embedded secrets and non-minimal attack surface (unnecessary
      packages, shells in production images)?
- [ ] Is there a rebuild/patch cadence for base images independent of application code changes
      (so a stale base image doesn't sit unpatched just because the app code hasn't changed)?
