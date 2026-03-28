# SEEK Scraper

Extract structured data from [seek.com.au](https://seek.com.au) — job listings from seek.com.au (Australia) and seek.co.nz (New Zealand). Get title, company, salary, location, description, and company profiles. Supports incremental tracking and compact output for AI agents.

**[Run on Apify →](https://apify.com/blackfalcondata/seek-scraper)**

---

## Key features

🔍 **Smart search with filters**

Search by keyword, location, and multiple filters. Smart input resolution ensures you always get results.

📄 **Detail enrichment**

Fetch full job descriptions, salary data, employer profiles, and contact information for each listing.

🔄 **Incremental mode**

Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

⚡ **Compact output for AI agents**

Core-fields-only mode optimized for MCP and AI agent workflows. Description truncation to control output size.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from seek.com.au on a schedule. Export to CSV, JSON, or directly to your database.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from seek.com.au.

**AI and LLM workflows**
Use compact mode and description truncation to feed data into AI agents, MCP servers, and LLM pipelines without exceeding token budgets.

---

## Quick start

```json
{
  "query": "software engineer",
  "maxResults": 50,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job search keywords (e.g. "software engineer", "accountant"). Use JSON array for multi-query. |
| `country` | enum | `"AU"` | Which SEEK market to search. |
| `location` | string | — | City, state, or region (e.g. "Sydney", "Melbourne VIC"). Use JSON array for multi-location. |
| `startUrls` | array | — | Direct SEEK search or job detail URLs. Overrides query/location. |
| `maxResults` | integer | `50` | Maximum total job listings to return. 0 = unlimited. |
| `maxPages` | integer | `25` | Maximum SERP pages to scrape per search source. Each page has ~22 jobs. |
| `sortMode` | enum | `"relevance"` | Sort search results by relevance or date. |
| `dateRange` | enum | — | Filter by posting date (SEEK date range code, e.g. "1" = last 24h, "3" = last 3 days, "7" = last 7 days, "14" = last 14 days, "31" = last 31 days). |
| `workType` | enum | — | Filter by employment type. |
| `workArrangement` | enum | — | Filter by work arrangement. |
| `includeDetails` | boolean | `true` | Fetch each job's detail page for full description, structured location, salary data, company profile, and more. |
| `descriptionMaxLength` | integer | `0` | Truncate job description to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Output only core fields (jobId, title, company, location, salary, category, URL, dates). Ideal for AI-agent and MCP workflows. |
| `incrementalMode` | boolean | `false` | Track changes between runs. Emits NEW, UPDATED, EXPIRED events. Requires stateKey. |
| `stateKey` | string | — | Stable identifier for the tracked search (e.g. "au-software-sydney"). Required when incrementalMode is true. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit records that haven't changed since last run. |
| `emitExpired` | boolean | `false` | When incremental, also emit records no longer found in search results. |

---

## FAQ

<!-- WRITE: 4-6 Q&A pairs relevant to this product -->

**Is it legal to scrape seek.com.au?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check the target site's terms of service for your specific use case.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage.

---

## Known limitations

<!-- WRITE: 4-6 honest limitations -->

- <!-- WRITE: limitation 1 -->
- <!-- WRITE: limitation 2 -->

---

## Related products by Black Falcon Data

| Product | Description |
|:--------|:------------|
| [StepStone Jobs API](https://github.com/BlackFalconData-org/stepstone-jobs-api) | Job listings from 18 European portals |
| [Company Jobs Tracker](https://github.com/BlackFalconData-org/company-jobs-tracker-api) | Track new/removed jobs per company |
| [Indeed Jobs Feed](https://github.com/BlackFalconData-org/indeed-jobs-feed) | Indeed job listings with salary data |
| [Glassdoor Jobs Feed](https://github.com/BlackFalconData-org/glassdoor-jobs-feed) | Glassdoor listings with company ratings |
| [Arbeitsagentur Jobs Feed](https://github.com/BlackFalconData-org/arbeitsagentur-jobs-feed) | Germany's federal job portal (1M+ listings) |
| [Naukri Jobs Feed](https://github.com/BlackFalconData-org/naukri-jobs-feed) | India's largest job portal |
| [Bilbasen Scraper](https://github.com/BlackFalconData-org/bilbasen-scraper) | Denmark's largest car marketplace |

---

*Last updated: 2026 03*
