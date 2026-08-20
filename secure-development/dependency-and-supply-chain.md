# Dependency & Software Supply Chain Security

Covers first-party CI/CD trust already handled in [`ci-cd/pipeline-security.md`](../ci-cd/pipeline-security.md);
this doc is about what gets pulled in from outside.

## Dependency intake

- [ ] Is there an internal package registry/proxy (rather than direct-from-public-registry
      installs) so a compromised upstream package can be caught/pinned before it reaches builds?
- [ ] Is there a policy for adding new dependencies — even lightweight (e.g., "check
      maintenance activity + license before adding a new dependency to a core service") — or is it
      unrestricted?
- [ ] Are transitive dependencies visible, not just direct ones? Most real supply-chain exposure
      is transitive.
- [ ] Is there a license compliance check integrated, to catch copyleft/incompatible licenses
      before they ship?

## SCA (software composition analysis)

- [ ] Is SCA scanning running on every build/PR, not just periodically?
- [ ] Is there a defined SLA for acting on new CVEs by severity, tied into the
      [vulnerability lifecycle](../vulnerability-management/lifecycle.md)?
- [ ] Is reachability/exploitability considered before prioritizing (a critical CVE in an unused
      code path is not the same urgency as one in a reachable path) — avoids flooding engineering
      with noise that erodes trust in the tool?
- [ ] Are dev-only dependencies distinguished from production dependencies in risk scoring?

## SBOM

- [ ] Is an SBOM (CycloneDX or SPDX) generated per build/release?
- [ ] Is the SBOM stored/versioned so "what was in production on date X" is answerable — critical
      for incident response when a new supply-chain CVE drops (e.g., a Log4Shell-class event)?
- [ ] Can the SBOM be queried quickly across the whole estate ("which services use package X at
      version < Y") rather than requiring a manual repo-by-repo search?

## Provenance & integrity

- [ ] Are build artifacts signed, and is signature verification enforced at deploy time?
- [ ] Where feasible, is SLSA-style provenance (or equivalent) generated for build artifacts?
- [ ] Are lockfiles committed and enforced (no floating version ranges resolving unpredictably at
      build time) for production services?
- [ ] Is there protection against dependency confusion (internal package names that could collide
      with a public registry namesquat)?

## Vendor / third-party SaaS

- [ ] Is there a lightweight vendor security review process before engineering adopts a new SaaS
      tool with access to code, data, or infrastructure?
- [ ] Is there an inventory of what third-party services have OAuth/API access into core systems
      (source control, cloud provider, data warehouse), and is it reviewed periodically for
      unused/over-scoped grants?

## Incident response for supply-chain events

- [ ] When a major upstream CVE breaks (transitive or direct), is there a documented fast path:
      query SBOM inventory → identify exposure → patch/mitigate → verify — rather than reinventing
      the process under pressure each time?
- [ ] Is there a designated owner for triggering this process the moment a high-profile CVE is
      announced, rather than waiting for scanner results to surface it hours/days later?
