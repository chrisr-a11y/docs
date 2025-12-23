# Jeff Feedback Implementation Plan

Based on feedback in `jeff_feedback.md`.

---

## 1. Add Rate Limit Warnings to Polling APIs

**Problem:** Polling-based APIs (like `GetTradeStats`) strain the database. JUMP is currently using these incorrectly.

**Solution:** Add clear warnings on polling APIs that have streaming equivalents.

### Tasks:
- [ ] Identify all polling APIs that have streaming counterparts
- [ ] Add warning callouts to each polling API page with:
  - Statement that low rate limits may be imposed at any time
  - Link to the corresponding streaming API
  - Recommendation to use snapshot+streaming pattern
- [ ] Consider adding a severity indicator (e.g., Mintlify `<Warning>` component)

### Example Warning Text:
```
<Warning>
This polling endpoint is subject to strict rate limits. For production use,
use the streaming API instead: [Link to streaming equivalent].
See our [Streaming First Architecture](/streaming) guide.
</Warning>
```

---

## 2. Link Individual API Docs to Streaming Equivalents

**Problem:** Developers skip to API reference pages and miss the "streaming first" guidance.

**Solution:** Add streaming alternative callouts on each individual polling API page.

### Tasks:
- [ ] Audit all REST API endpoints to identify which have streaming equivalents
- [ ] Create a mapping table: REST endpoint → gRPC streaming equivalent
- [ ] Add "Streaming Alternative" section or callout to each applicable REST API page
- [ ] Link directly to the specific streaming API, not just the general streaming docs page

---

## 3. Bifurcate ISV vs Trading APIs

**Problem:** Unclear which APIs are for ISV partners (providing retail trader access) vs direct trading APIs.

**Solution:** Reorganize documentation hierarchy to separate ISV and Trading sections.

### Tasks:
- [ ] Identify which APIs are ISV-specific vs Trading-specific
- [ ] Create separate navigation sections in `mint.json`:
  - "Trading APIs" - for direct trading partners
  - "ISV Partner APIs" - for partners providing retail access
- [ ] Add introductory pages for each section explaining the intended use case
- [ ] Update navigation structure to reflect the bifurcation
- [ ] Add guidance on which section applies to which partner type

### Proposed Structure:
```
REST API
├── Trading APIs
│   ├── Orders
│   ├── Positions
│   ├── Order Book
│   └── Drop Copy
└── ISV Partner APIs
    ├── Accounts
    ├── KYC
    ├── Payments
    └── [Other ISV-specific endpoints]
```

---

## 4. Fix Message Flow Diagrams - Consolidate Actors

**Problem:** Diagrams show "Polymarket Gateway" and "Polymarket US" as separate actors, exposing internal architecture.

**Solution:** Consolidate all internal services under single "Polymarket US" actor.

### Tasks:
- [ ] Locate all message flow diagrams in the documentation
- [ ] Update diagrams to replace:
  - "Polymarket Gateway" → "Polymarket US"
  - Any other internal service names → "Polymarket US"
- [ ] Ensure diagrams only show: Client ↔ Polymarket US
- [ ] Review any sequence diagrams for similar issues

---

## 5. Rename "Auth0 Access Token" Terminology

**Problem:** "Auth0 Access Token" exposes implementation details. External users don't need to know we use Auth0.

**Solution:** Rename to "Polymarket US API Access Token" throughout documentation.

### Tasks:
- [ ] Search all docs for "Auth0" references
- [ ] Replace terminology:
  - "Auth0 Access Token" → "Polymarket US API Access Token" (or "API Access Token")
  - "Auth0 JWT" → "API Access Token"
- [ ] Update authentication guide with cleaner terminology
- [ ] Keep any necessary Auth0 references only in internal/implementation notes (if any)

### Search patterns:
- `Auth0`
- `auth0`
- `AUTH0`

---

## ~~6. Migrate to Auth Custom Domain~~ ✅ DONE

Auth custom domain infrastructure is in place. Documentation updated to use:

| Environment | Auth Domain |
|-------------|-------------|
| Production  | `auth.polymarketexchange.com` |
| Preprod     | `auth.preprod.polymarketexchange.com` |
| Staging     | `auth.staging.polymarketexchange.com` |
| Dev         | `auth.dev.polymarketexchange.com` |

---

## Implementation Order

1. ~~**Auth custom domain (#6)**~~ ✅ DONE
2. **Terminology changes (#5)** - Quick find/replace, low risk
3. **Diagram fixes (#4)** - Isolated changes to diagram files
4. **Rate limit warnings (#1)** - Add callouts to existing pages
5. **Streaming links (#2)** - Requires mapping exercise first
6. **ISV bifurcation (#3)** - Largest structural change, do last

**Note:** #5 (terminology) should be done next to complete Auth0 branding removal.

---

## Files Likely Affected

- `mint.json` - Navigation structure
- `api-reference/**/*.mdx` - API endpoint documentation
- `introduction.mdx` or similar - Overview pages
- Any files containing Mermaid/diagram definitions
- Authentication/getting-started guides

---

## Notes

- Changes should maintain backwards compatibility for existing API consumers
- Consider adding a "Deprecation" or "Not Recommended" tag for heavily-rate-limited polling endpoints
