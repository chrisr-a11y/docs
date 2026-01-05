# Jeff Feedback Implementation Plan

Based on feedback in `jeff_feedback.md`.

---

## ~~1. Add Rate Limit Warnings to Polling APIs~~ ✅ DONE

Added streaming-first warnings to polling APIs:
- Order Book API overview with warning and streaming recommendation table
- Report API overview with warning and use case table
- Drop Copy API overview with warning and streaming recommendation
- REST API introduction with prominent "Streaming First Architecture" section

---

## ~~2. Link Individual API Docs to Streaming Equivalents~~ ✅ DONE

Created overview pages for all major API groups with streaming alternative callouts:
- `api-reference/trading/overview.mdx`
- `api-reference/report/overview.mdx`
- `api-reference/positions/overview.mdx`
- `api-reference/orderbook/overview.mdx`
- `api-reference/refdata/overview.mdx`
- `api-reference/dropcopy/overview.mdx`
- `api-reference/accounts/overview.mdx`

Each overview includes use case tables showing when to use REST vs streaming.

---

## ~~3. Bifurcate ISV vs Trading APIs~~ ✅ DONE

Reorganized REST API navigation in `docs.json`:
- Added "Trading APIs" section header with overview
- Added "ISV Partner APIs" section header with overview
- Updated `api-reference/introduction.mdx` with clear Trading vs ISV Partner API guidance
- Added guidance on which APIs each partner type needs

---

## ~~4. Fix Message Flow Diagrams - Consolidate Actors~~ ✅ DONE

Updated all diagrams to use consistent actor names:
- Authentication diagrams now use "Polymarket US Auth" and "Polymarket US API"
- Removed references to "Polymarket Gateway" as separate actor
- All diagrams now show: Client ↔ Polymarket US (Auth/API)

---

## ~~5. Rename "Auth0 Access Token" Terminology~~ ✅ DONE

Removed Auth0 branding from all documentation:
- Renamed `auth0-onboarding.mdx` → `authentication.mdx`
- Updated all "Auth0 JWT" → "access token"
- Updated all "Auth0 Access Token" → "API access token"
- Updated diagrams to show "Polymarket US Auth" instead of "Auth0"
- Updated class names from `Auth0Client` → `AuthClient`
- Updated all internal links to new authentication page

---

## ~~6. Migrate to Auth Custom Domain~~ ✅ DONE

Auth custom domain infrastructure is in place. Documentation updated to use:

| Environment | Auth Domain |
|-------------|-------------|
| Production  | `pmx-prod.us.auth0.com` |
| Preprod     | `pmx-preprod.us.auth0.com` |
| Dev         | `pmx-dev.us.auth0.com` |

---

## Implementation Order

1. ~~**Auth custom domain (#6)**~~ ✅ DONE
2. ~~**Terminology changes (#5)**~~ ✅ DONE
3. ~~**Diagram fixes (#4)**~~ ✅ DONE
4. ~~**Rate limit warnings (#1)**~~ ✅ DONE
5. ~~**Streaming links (#2)**~~ ✅ DONE
6. ~~**ISV bifurcation (#3)**~~ ✅ DONE

**All items complete!**

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
