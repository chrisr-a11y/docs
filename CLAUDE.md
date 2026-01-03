# Polymarket Exchange Partner Docs

> **GitHub Account**: This repo uses the `chrisrodier-beep` account (default `gh auth`).
> No special token needed - uses default gh CLI authentication.

Mintlify documentation site for the Polymarket Exchange API.

## Publishing Setup

Documentation is published via Mintlify from the `docs-2026` repo:
- **Remote name:** `docs-2026`
- **Repo:** `git@github.com:Polymarket-US/website-dcm-docs-2026.git`
- **Branch:** `main`

**Publishing workflow (after making changes):**
```bash
# 1. Commit changes locally
git add .
git commit -m "docs: Your change description"

# 2. Create a branch and push to docs-2026
git checkout -b your-branch-name
git push -u docs-2026 your-branch-name

# 3. Create and merge PR to trigger Mintlify deployment
gh pr create --repo Polymarket-US/website-dcm-docs-2026 --title "docs: Your change" --body "Description" --base main
gh pr merge <PR_NUMBER> --repo Polymarket-US/website-dcm-docs-2026 --merge --admin
```

Merging to `docs-2026/main` triggers the Mintlify deployment automatically.

## API Documentation Structure

**REST API Tab** (uses OpenAPI schemas from `api-reference/oapi-schemas/`):
- Health - Service health check
- Accounts - User/account management
- Trading - Insert/cancel orders (OrderEntryAPI)
- Orders - Search orders/trades (OrderAPI)
- Positions - Balances and positions (REST only, not subscriptions)
- Reference Data - Instruments and symbols (RefDataAPI)
- Order Book - L2 snapshots, BBO
- KYC - Identity verification
- Payments - Aeropay, Checkout.com, Funding

**Connect API Tab** - Streaming endpoints via Connect protocol:
- Market Data Subscription
- Order Subscription
- Position Subscription
- Drop Copy (all 4 endpoints)

**gRPC API Tab** - Same streaming as Connect, but via gRPC

**FIX API Tab** - FIX protocol documentation

## Diagram Naming Convention

In mermaid sequence diagrams, use these participant names consistently:
- `Auth as Polymarket US Auth` - Authentication service
- `API as Polymarket US API` - API service
- `P as Polymarket US` - Short form when space is limited

Example:
```mermaid
sequenceDiagram
    participant Client
    participant Auth as Polymarket US Auth
    participant API as Polymarket US API
```

Do NOT use "Gateway" or other internal names in public documentation.

## URL Path Convention

The REST API uses `/v1/` paths (RESTful style):
- `/v1/trading/orders` - Insert order
- `/v1/trading/orders/cancel` - Cancel order
- `/v1/orders/search` - Search orders
- `/v1/refdata/instruments` - List instruments

Legacy `/v1beta1/` paths are supported for backwards compatibility but NOT documented.

## MDX Syntax Restrictions

Mintlify uses MDX (Markdown + JSX). Avoid these characters that break parsing:

**Do NOT use:**
- Curly braces `{ }` outside code blocks - interpreted as JSX expressions
- Angle brackets `< >` in text - interpreted as HTML/JSX tags
- Checkbox characters `☐ ☑` - cause parsing errors
- Other special Unicode symbols that may conflict with JSX

**Instead use:**
- Square brackets `[ ]` for placeholders: `[companyname]` not `{companyname}`
- Words for ranges: `Under $1M` not `<$1M`, `Over $100M` not `>$100M`
- Markdown checkboxes: `- [ ]` in lists, or just leave blank fields in tables

**Test locally before pushing:**
```bash
npx mintlify dev
```
Watch for "parsing error" messages in the terminal output.

