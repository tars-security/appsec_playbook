# API Security Checklist

Covers REST/GraphQL/RPC APIs, internal or external. Organized around OWASP API Security Top 10
categories but reframed as review questions rather than a restatement of the list.

## Authentication

- [ ] Is every endpoint's auth requirement explicit (no accidental unauthenticated routes from
      missing middleware/decorator)?
- [ ] For API keys/tokens: are they scoped (least privilege) rather than all-or-nothing access?
- [ ] Is there rate limiting on auth endpoints specifically (login, password reset, token refresh)
      to blunt credential stuffing / brute force?
- [ ] For machine-to-machine auth: short-lived tokens/workload identity preferred over static
      long-lived API keys.

## Authorization (broken object/function level authz — the most common real-world API finding)

- [ ] Object-level: does every endpoint accepting an ID verify the caller is authorized for *that*
      specific object, server-side, on every request?
- [ ] Function-level: are admin/privileged endpoints protected by role checks, not just obscurity
      (unlisted in docs ≠ protected)?
- [ ] For GraphQL specifically: is field-level authorization enforced (a broadly-scoped query
      shouldn't be able to traverse relationships into unauthorized data)?

## Data exposure

- [ ] Do responses return only the fields the client needs, not full internal object
      representations (mass assignment / excessive data exposure)?
- [ ] Is there a check against accidentally returning other users' data in list/search endpoints
      via missing filter-by-owner logic?
- [ ] For GraphQL: is introspection disabled or restricted in production if the schema exposes
      sensitive internal structure?

## Rate limiting & resource consumption

- [ ] Is there rate limiting per-user/per-key (not just global) to prevent one client exhausting
      shared capacity?
- [ ] Are pagination limits enforced server-side (can't request unbounded page sizes)?
- [ ] For GraphQL: is query depth/complexity limited to prevent expensive nested query abuse?
- [ ] Are file upload endpoints size- and rate-limited?

## Input validation & injection

- [ ] Is all input validated server-side regardless of client-side validation (never trust client
      validation as the only layer)?
- [ ] Are batch/bulk endpoints validated per-item, not just at the envelope level?
- [ ] Is content-type strictly enforced (reject unexpected types rather than best-effort parsing)?

## Inventory & lifecycle

- [ ] Is there a maintained API inventory (formal spec — OpenAPI/GraphQL schema — as source of
      truth), or does "shadow API" risk exist from undocumented endpoints?
- [ ] Are deprecated/old API versions actually decommissioned, or left reachable indefinitely
      (old versions often lack newer security controls)?
- [ ] Is there a process for retiring internal/debug endpoints before production deploy?

## Third-party & server-side request risk

- [ ] For any endpoint accepting a URL/webhook target from the caller: is SSRF mitigated
      (allowlist destinations, block internal/metadata IP ranges, disable redirects to internal
      hosts)?
- [ ] Are outbound webhook payloads signed so receivers can verify authenticity?

## Logging & monitoring

- [ ] Are authentication failures, authorization failures, and rate-limit hits logged with enough
      context to detect abuse patterns?
- [ ] Is there alerting on anomalous API usage (sudden volume spike from one key, sequential ID
      enumeration pattern)?

## Testing

- [ ] Are API fuzzing / contract-based security tests part of CI for high-risk services, or only
      covered by periodic manual pentest?
- [ ] Does the test suite include negative authz tests (user A cannot access user B's resource) as
      a standing regression check, not just a one-time pentest finding fix?
