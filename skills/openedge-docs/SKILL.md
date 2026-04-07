---
name: openedge-docs
description: "Look up OpenEdge documentation URLs, version mappings, and bundle name patterns on docs.progress.com. Use when searching for or referencing OpenEdge customer-facing documentation by version."
---

# OpenEdge Documentation Lookup

## When to Use

Activate this skill when you need to:
- Find the correct docs.progress.com URL for an OpenEdge version
- Construct search queries scoped to a specific documentation set
- Identify which documentation bundle covers a given topic area
- Validate that a docs.progress.com URL is current

## Trusted Documentation Source

- Treat **OpenEdge Information Hub** as the primary trusted source:
  `https://docs.progress.com/category/openedge-information-hub`
- All OpenEdge bundles linked from or discoverable via this hub are trusted.
- Start documentation discovery from the Information Hub, then follow version/category
  links to identify exact pages.

## Version → URL Mapping

| Version | Category URL | Bundle Suffix | Search Scope |
|---|---|---|---|
| **12.2** | `docs.progress.com/category/openedge122` | `-122` | `site:docs.progress.com openedge 12.2` |
| **12.8** | `docs.progress.com/category/openedge-information-hub` | none or `-128` | `site:docs.progress.com openedge 12.8` |
| **13.0** | `docs.progress.com/category/openedge` | `-13` | `site:docs.progress.com openedge 13.0` |

## Bundle Name Patterns

Append the version suffix from the table above to construct full bundle names:

| Bundle Pattern | Topic Area |
|---|---|
| `openedge-whats-new` | What's New / Release highlights |
| `openedge-abl-reference` | ABL Language Reference |
| `openedge-abl-devel` | ABL Development Guide |
| `openedge-database-admin` | Database Administration |
| `openedge-pas` | PAS for OpenEdge (Pacific Application Server) |
| `openedge-security` | Security Guide |
| `openedge-deployment` | Deployment Guide |
| `openedge-product-notes` | Product Notes / Known Issues *(not maintained by Info Dev — exclude from stories)* |
| `openedge-getting-started` | Getting Started |
| `openedge-management-introduction` | Management & Monitoring |
| `openedge-upgrade` | Upgrade Guide |
| `openedge-life-cycle` | Life Cycle / Support Matrix |

## Search Strategy

For each version, run searches in this order:

1. **Keyword search**: `site:docs.progress.com {keyword} openedge {version}`
2. **Component search**: `site:docs.progress.com {component-name} openedge {version}`
3. **Broadened search** (if version-scoped results are sparse): `site:docs.progress.com {keyword} openedge`

## Page Retrieval Notes

- docs.progress.com is a JavaScript-rendered Zoomin SPA.
- `fetch_webpage` may return empty content. If this happens:
  - Rely on page title, URL path, and search snippet for context.
  - Use web search to find cached or indexed descriptions.
  - State clearly when page content could not be verified.

## Version Coverage Rules

- Assess each listed version independently.
- Do not assume content parity across versions — the same topic may differ between 12.2, 12.8, and 13.0.

## Referenced URL Validation

When a Jira ticket includes a specific docs.progress.com URL, classify it as:
- **Current valid page** — page loads and content is current
- **Redirect or successor topic found** — URL redirects to a different page
- **Retired or 404** — page no longer exists; identify the best current replacement topic
