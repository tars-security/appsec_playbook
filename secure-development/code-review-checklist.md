# Secure Code Review Checklist

Applies to PRs flagged high-risk: auth/authz changes, payments, data access layer, file
upload/parsing, deserialization, cryptography, third-party integrations, admin/internal tooling.
Not intended as a checklist for every PR — that dilutes attention and slows everyone down for no
benefit.

## Triggering a security review

Define explicit trigger criteria (file path patterns, PR labels, or team convention) rather than
relying on engineers to remember to ask — e.g., any diff touching `auth/`, `payments/`,
`**/migrations/`, or files importing a deserialization/crypto library auto-tags for review.

## Authentication & session management

- [ ] Are credentials/tokens compared with constant-time comparison where applicable?
- [ ] Do session tokens rotate on privilege change (login, password reset, role change)?
- [ ] Is there a server-side session/token invalidation path (logout actually invalidates, not
      just clears client state)?
- [ ] Are password/secret fields excluded from logs, error messages, and serialized responses?

## Authorization

- [ ] Is authorization checked on every access path to the resource, not just the "main" one
      (e.g., also checked on the export/bulk endpoint, not just the single-record endpoint)?
- [ ] Are object-level checks present (IDOR) — does the code verify the requester owns/can access
      *this specific* resource ID, not just that they're authenticated?
- [ ] Does authorization logic live server-side only, not inferred from client-supplied role/flag
      fields?
- [ ] For multi-tenant systems: is tenant isolation enforced at the query level (not just at the
      UI), and is there a test proving cross-tenant access fails?

## Input handling

- [ ] Are all external inputs (params, headers, body, file contents) validated against an
      allowlist where feasible, rather than a denylist?
- [ ] Is user input reaching a query, shell command, or template interpolation via
      parameterization/escaping (not string concatenation)?
- [ ] Are file uploads validated by content, not just extension/MIME header, and stored outside
      the web root or in object storage with no execute permission?
- [ ] Is deserialization restricted to safe formats/types (no arbitrary object deserialization
      from untrusted input)?

## Data handling

- [ ] Is sensitive data (PII, secrets, tokens) encrypted at rest where required, and excluded from
      logs/traces/error reporting?
- [ ] Are responses scoped to only the fields the caller needs (no accidental over-fetch/serialize
      of internal-only fields)?
- [ ] Do database queries use least-privilege credentials appropriate to the operation?

## Cryptography

- [ ] Are only vetted, current libraries/algorithms used (no custom crypto, no deprecated
      algorithms/modes)?
- [ ] Are secrets/keys pulled from a secrets manager, not hardcoded or committed?
- [ ] Is randomness for security-sensitive values (tokens, reset codes) from a CSPRNG, not a
      standard PRNG?

## Error handling & logging

- [ ] Do error responses avoid leaking stack traces, internal paths, or query details to the
      client?
- [ ] Are security-relevant events (auth failures, permission denials, admin actions) logged with
      enough context to investigate later?
- [ ] Do logs avoid capturing secrets, tokens, or full PII payloads?

## Dependencies & configuration

- [ ] Are new dependencies checked against
      [`secure-development/dependency-and-supply-chain.md`](dependency-and-supply-chain.md)
      before merge?
- [ ] Are any new configuration flags/feature flags defaulting to the secure state?
- [ ] Does the change introduce a new trust boundary or external integration that should trigger
      [threat modeling](threat-modeling.md) if it hasn't already?

## Reviewer notes

- A security reviewer's job is to find the one thing the standard code reviewer wouldn't catch —
  don't duplicate the functional review.
- Where a finding isn't a blocker but should be tracked, file it rather than leaving it as an
  unresolved PR comment thread — comments get lost, tickets don't.
- If the same class of finding recurs across PRs from the same team, that's a signal for a
  lint rule / SAST custom rule rather than repeated manual review — automate the catch.
