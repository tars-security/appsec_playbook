# Scanner Depth Audit — SAST Rules, DAST, Fuzzing, Container/Image Scanning

Companion to [`pipeline-security.md`](pipeline-security.md). That doc covers whether scanning is
*integrated* (placement, gating, routing); this one covers whether the scanning that exists
actually produces trustworthy signal. A tool being "installed" and a tool being "worth trusting
enough to gate on" are different questions — this audit answers the second one, typically once
per tool per year or whenever coverage is in question.

## SAST rule audit

- [ ] What ruleset(s) are active — vendor default only, or extended with org-specific custom
      rules?
- [ ] For custom rules: are they mapped to the org's actual top vulnerability classes (from
      pentest/bug bounty/incident history), or written generically without that feedback loop?
- [ ] What's the measured false positive rate? Sample recent findings marked
      suppressed/resolved-not-fixed rather than assuming the vendor's marketing number.
- [ ] Is there a documented suppression process, and is the suppression list itself reviewed
      periodically — stale suppressions silently hide real regressions in the same spot.
- [ ] Does the engine do taint/data-flow analysis for the languages in use, or pattern-matching
      only (materially different depth of signal, worth knowing which you have)?
- [ ] When was the ruleset/engine last updated? Rules go stale as frameworks evolve — e.g.,
      injection rules that don't recognize a newer ORM's query API.
- [ ] Is coverage even across the stack, or only wired up for the primary language, leaving
      secondary languages/services dark?
- [ ] Who owns rule tuning, and is there a cadence — or does tuning only happen reactively when
      noise complaints pile up?

## DAST audit

- [ ] Does DAST exist at all today? If not, that absence is itself the finding.
- [ ] What's the scan target — staging, prod, or both — and is scanning authenticated or
      unauthenticated?
- [ ] If authenticated: does the scan actually reach logged-in/business-logic flows, or stall at
      the pre-auth surface? Most DAST value is lost if it can't get past login.
- [ ] What's the cadence — continuous/per-build, or manually triggered and easily forgotten?
- [ ] Do DAST findings route into the same [vulnerability lifecycle](../vulnerability-management/lifecycle.md)
      as everything else, or sit in a separate dashboard nobody checks?
- [ ] Does DAST coverage meaningfully overlap with what pentests keep finding? If pentests
      repeatedly catch DAST-detectable bug classes, DAST isn't running effectively or isn't tuned
      to the app.

## Fuzzing audit

- [ ] Does any fuzzing exist — API fuzzing, protocol fuzzing, or unit-level fuzz targets?
- [ ] Is API fuzzing schema-driven (generated from the OpenAPI/GraphQL schema, tracking the real
      surface as it evolves) or hand-maintained and prone to drifting stale?
- [ ] Does it cover state-changing endpoints (POST/PUT/DELETE/mutations), or only safe GETs —
      most real bugs live in state-changing paths.
- [ ] Does it run continuously in CI/on a schedule, or was it a one-off exercise never repeated?
- [ ] Is there a triage process routing fuzzer-found crashes/anomalies into the standard
      vulnerability lifecycle, or do they get lost in CI logs?
- [ ] Is there a corpus/seed-case strategy carried between runs, or does every run start cold —
      materially reduces bug-finding depth over time?

## Container/image scanning audit

- [ ] Is scanning done pre-push (local/CI, fast feedback) or only at registry/pre-deploy — or
      both? Registry-only means a vulnerable image can sit in a dev/staging loop unflagged for a
      while.
- [ ] What's the enforced CVE severity threshold for blocking, and does it account for
      reachability (a critical CVE in an unused library path is not the same risk as one in the
      request path)?
- [ ] Is the base image pinned and tracked, or does `latest`/floating tags mean the same
      Dockerfile silently produces a different risk profile build to build?
- [ ] How stale is the base image itself — is there a process to rebuild/republish images when the
      base image gets patched, independent of any app code change?
- [ ] Does the scanner cover OS packages and language-level dependencies inside the image, or only
      one of the two?
- [ ] Are images scanned for embedded secrets and unnecessary attack surface (shells, package
      managers, debug tooling left in a production image)?

## Output

Score each of the four areas against the same 0–3 scale used in
[`discovery/maturity-assessment.md`](../discovery/maturity-assessment.md) and feed the result back
into that assessment's relevant rows. A tool that "exists" but scores low here is a common false
sense of coverage — call it out explicitly to leadership rather than letting "we have a scanner"
stand in for "we have signal." This is also the gating decision input: don't move a scanner from
advisory to blocking (see [`pipeline-security.md`](pipeline-security.md#gating-policy)) until it
scores at least "developing" here.
