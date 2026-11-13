# Awesome-Quickbooks-Sync-GL-Integration

# Awesome QuickBooks Sync & GL Integration

> A curated list of top QuickBooks Sync and GL Integration platforms — both SaaS/Hosted commercial connectors and open-source alternatives for syncing invoices, payments, and chart of accounts between QuickBooks and external systems.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [Chargebee QuickBooks Integration](#chargebee-quickbooks-integration)
  - [Celigo QuickBooks Connector](#celigo-quickbooks-connector)
  - [Zapier QuickBooks Sync](#zapier-quickbooks-sync)
  - [Make QuickBooks App](#make-quickbooks-app)
  - [Workato QuickBooks Connector](#workato-quickbooks-connector)
- [Open-Source Software](#-open-source-software)
  - [LedgerSync](#ledgersync)
  - [django-quickbooks-sync](#django-quickbooks-sync)
  - [python-quickbooks](#python-quickbooks)
  - [local-books](#local-books)
  - [SlowBooks-Pro-2026](#slowbooks-pro-2026)
- [Comparison at a Glance](#-comparison-at-a-glance)
- [Contributing](#-contributing)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted QuickBooks Sync and GL Integration platforms. These are not open-source but are included as reference points for feature comparison.

### Chargebee QuickBooks Integration
**Type:** Billing-to-GL sync with account mapping
Chargebee's QuickBooks integration syncs invoices, payments, and credits to QuickBooks Online with configurable GL account mapping. Plans and addons can be mapped to specific GL accounts, with separate mappings for payments, refunds, round-offs, one-time charges, setup fees, and discounts [citation:11]. **Important caveat:** If you change a GL Account Name in QuickBooks after setup, you must update it in Chargebee as well, or sync errors may occur [citation:11]. A prerequisite check validates currency alignment and QuickBooks preferences (such as disabling "Automatically Apply Credits" and enabling "Custom Transaction Numbers") before sync begins [citation:11]. Stripe reconciliation is supported if Stripe is the gateway.
**Best for:** Chargebee customers wanting native invoice and payment sync to QuickBooks Online with granular GL mapping, accepting manual maintenance of account name changes.

### Celigo QuickBooks Connector
**Type:** Integration platform with prebuilt QuickBooks flows
Celigo's integrator.io provides prebuilt integration templates for QuickBooks, including QuickBooks-to-NetSuite migration flows for customers, employees, invoices, orders, and products [citation:12]. The Free Edition allows one integration flow at a time in perpetuity, with a 30-day trial for unlimited flows [citation:5]. Templates include prebuilt mappings and a NetSuite bundle (341276) with custom fields and subtabs for holding QuickBooks data during migration [citation:5]. Flows can be customized and run on a schedule or on demand [citation:5].
**Best for:** Organizations migrating from QuickBooks to NetSuite or needing prebuilt, configurable flows between QuickBooks and other systems, accepting that the free tier runs one flow at a time.

### Zapier QuickBooks Sync
**Type:** No-code automation with QuickBooks triggers and actions
Zapier connects QuickBooks to 7,000+ apps with trigger-action workflows. Common use cases include creating QuickBooks invoices from new form submissions, syncing payments to spreadsheets, and triggering notifications on new customers. It is a lightweight glue layer rather than a dedicated GL integration platform, with per-task pricing that can scale.

### Make QuickBooks App
**Type:** Visual automation with QuickBooks modules
Make provides QuickBooks modules within its visual scenario builder, allowing multi-step workflows with branching, iteration, and data transformation. It offers more granular data control than Zapier, making it suitable for complex sync logic between QuickBooks and other systems.

### Workato QuickBooks Connector
**Type:** Enterprise automation with QuickBooks recipes
Workato provides QuickBooks connectors with prebuilt recipes for customer sync, invoice creation, and payment updates. It is positioned for enterprises wanting governance, audit trails, and cross-enterprise visibility over their QuickBooks integrations.

---

## 🟩 Open-Source Software

Self-hosted, community-driven QuickBooks Sync and GL Integration tools. The open-source ecosystem spans **multi-platform sync libraries**, **Django-specific bidirectional sync**, **API client libraries**, and **local mirroring tools**.

### LedgerSync
**Stack:** Ruby | **License:** Open Source (per repository)
LedgerSync is an open-source Ruby library for syncing data to QuickBooks, Xero, NetSuite, and more [citation:1]. It provides a programmatic abstraction layer for financial data synchronization, useful for developers building custom integrations or embedded accounting workflows. Documentation is available at ledgersync.dev [citation:1].
**Best for:** Ruby developers building custom sync workflows between applications and QuickBooks, accepting a library-level integration rather than a turnkey platform.

### django-quickbooks-sync
**Stack:** Python / Django / Celery | **License:** Open Source (PyPI)
django-quickbooks-sync is a production-grade Django integration for QuickBooks Online with bidirectional sync, OAuth 2.0 management, and audit logging [citation:3]. Key features include:
- **Bidirectional sync** between Django models and QuickBooks entities (Account, Customer, Vendor, Employee, Invoice, Bill, Payment, Item)
- **Celery integration** for async processing with retry logic
- **Rate limiting** compliance (500 req/min per realm, 10 concurrent) with exponential backoff
- **Idempotency** to prevent duplicate operations
- **Conflict resolution** strategies: `last_write_wins` (default), `source_wins`, or `manual`
- **Webhook support** for real-time event processing
- **Multi-realm** support for multiple QuickBooks companies

Management commands include `quickbooks_setup --interactive`, `quickbooks_sync --all-realms`, and `quickbooks_webhook --setup` [citation:3]. Setup requires configuring client ID, secret, redirect URI, and webhook verifier token in Django settings.
**Best for:** Django developers wanting a production-grade, bidirectional QuickBooks sync with conflict resolution and Celery-based async processing, accepting the Django/Celery stack requirements.

### python-quickbooks
**Stack:** Python | **License:** Open Source (PyPI)
python-quickbooks is the most established Python library for accessing the QuickBooks API, with **430 GitHub stars** and **205,000 monthly downloads** [citation:2]. It provides a Pythonic interface to QuickBooks Online entities including customers, invoices, accounts, and payments. It is a client library rather than a sync tool—you build the sync logic on top.
**Best for:** Python developers needing a mature, widely adopted client library for building custom QuickBooks integrations.

### local-books
**Stack:** Bun / TypeScript / SQLite | **License:** Open Source (per repository)
local-books is a local, re-pullable mirror of QuickBooks data stored in SQLite, designed for AI agent access without sending financial data to the cloud [citation:15]. Key features include:
- **Local SQLite mirror** per company (realmId), with one table per QuickBooks entity storing raw JSON plus extracted scalar columns [citation:8]
- **CDC-based sync** with a 30-day lookback window; past that, a full pull is forced [citation:8]
- **Soft deletes only**—CDC deletes mark `deleted = 1`, preserving the local copy as the last surviving record [citation:8]
- **Read-only SQL access** via `local-books query`, plus live QuickBooks reports via `local-books report` [citation:10]
- **MCP server** (`local-books mcp`) so Claude Code or Codex can query the books through tools rather than hand-written SQL [citation:15]
- **Tokens stored separately** in `credentials.json` at `0600` permissions, never inside the company mirror [citation:8]

Auth uses a localhost OAuth callback, with separate Development (sandbox) and Production keysets [citation:15]. Rate limits are respected (500 req/min, 10 concurrent) with 429 backoff [citation:8].
**Best for:** Developers wanting a local, AI-queryable mirror of QuickBooks data without cloud dependency, accepting a CLI-driven workflow and manual OAuth app setup with Intuit.

### SlowBooks-Pro-2026
**Stack:** Python | **License:** Open Source
SlowBooks-Pro-2026 is an open-source, **QuickBooks 2003 Pro replacement** for personal and enterprise bookkeeping, decompiled from QBW32.EXE [citation:14]. It provides double-entry bookkeeping, invoicing, US payroll, inventory, and bank feeds as a desktop app with a built-in multi-user Server Edition (no Docker required) [citation:7]. It is a full accounting application rather than a sync tool, with 205 GitHub stars and active maintenance [citation:14].
**Best for:** Organizations wanting a free, local-first QuickBooks 2003 replacement with full bookkeeping capabilities, accepting that it is a standalone accounting app rather than a QuickBooks integration.

---

## 📊 Comparison at a Glance

| Software | License | Stack | Sync Direction | GL/Account Mapping | Self-Hosted | Best For |
|----------|---------|-------|----------------|---------------------|-------------|----------|
| **Chargebee** | Proprietary | Cloud | Billing → QBO | ✅ Granular | ❌ | Native invoice sync + GL mapping |
| **Celigo** | Proprietary | Cloud | Bi-directional | ✅ Configurable | ❌ | Prebuilt migration flows |
| **Zapier** | Proprietary | Cloud | Bi-directional | (Custom) | ❌ | No-code app connectivity |
| **LedgerSync** | Open Source | Ruby | Bi-directional | (Library) | ✅ | Ruby custom sync |
| **django-quickbooks-sync** | Open Source | Django/Celery | Bi-directional | ✅ Entity sync | ✅ | Django production sync |
| **python-quickbooks** | Open Source | Python | (Client only) | ❌ | ✅ | Python API client |
| **local-books** | Open Source | Bun/TypeScript | QBO → Local | (Raw JSON) | ✅ | Local AI-queryable mirror |
| **SlowBooks** | Open Source | Python | N/A (standalone) | N/A | ✅ | QuickBooks 2003 replacement |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source QuickBooks Sync or GL Integration tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source. Note that the open-source ecosystem **splits** between **sync libraries for developers** (LedgerSync for Ruby, django-quickbooks-sync for Django, python-quickbooks as a client foundation) and **local mirroring tools for AI access** (local-books). **django-quickbooks-sync** is the most complete open-source bidirectional sync package with conflict resolution and Celery-based async processing [citation:3]. **local-books** takes a fundamentally different approach—a local SQLite mirror with MCP server access for AI agents, keeping financial data entirely on-device [citation:15]. Commercial platforms like Chargebee and Celigo remain the practical choice for production GL sync with prebuilt mappings and vendor support.

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐
