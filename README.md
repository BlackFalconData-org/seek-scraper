# SEEK Scraper

Extract structured data from [seek.com.au](https://seek.com.au) — job listings from seek.com.au (Australia) and seek.co.nz (New Zealand). Get title, company, salary, location, description, and company profiles. Supports incremental tracking and compact output for AI agents.

**[SEEK Job Scraper - Australia & New Zealand Job Listings on Apify →](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi)**

---

## Key features





**Search with filters** — Search by keyword and location. Filter by country, sort by, date range, and more.

**Detail enrichment** — Fetch full job descriptions, salary data for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings (up to 5.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases





**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from seek.com.au on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from seek.com.au.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on seek.com.au to inform pricing decisions, hiring plans, or candidate negotiations.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

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


## Output fields

Every listing returns the same 58-field schema. Missing values are `null` — never omitted.

- `jobId`
- `seekJobId`
- `title`
- `canonicalUrl`
- `company`
- `companyUrl`
- `advertiserId`
- `location`
- `locationCountry`
- `locationState`
- `locationSuburb`
- `locationPostcode`
- `salaryText`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryType`
- `employmentType`
- `workArrangement`
- `category`
- `subCategory`
- `roleId`
- `teaser`
- `bulletPoints`
- `description`
- `descriptionHtml`
- `descriptionMarkdown`
- `descriptionLength`
- `contentQuality`
- `companyIndustry`
- `companySize`
- `companyWebsite`
- `phoneNumber`
- `screeningQuestions`
- `applyUrl`
- `postedDate`
- `validThrough`
- `contentHash`
- `isSponsored`
- `sourceUrl`
- `sourceCountry`
- `sourceDomain`
- `searchQuery`
- `searchUrl`
- `scrapedAt`
- `fetchedAt`
- `detailFetched`
- `extractedEmails`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`
- `changeType`
- `trackedHash`
- `firstSeenAt`
- `lastSeenAt`
- `previousSeenAt`
- `expiredAt`
- `stateKey`


## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "jobId": "cdce3e87d731956375b8bb69a41dd68c111b49ba0030ef74b0d0502051f7df36",
  "seekJobId": "91576706",
  "title": "Software Engineer – AWS DevOps",
  "canonicalUrl": "https://www.seek.com.au/job/91576706",
  "company": "Paperless Warehousing Pty Ltd",
  "companyUrl": null,
  "advertiserId": "63532852",
  "location": "Melbourne VIC",
  "locationCountry": "AU",
  "locationState": "Victoria",
  "locationSuburb": "Melbourne",
  "locationPostcode": "3000"
}
```

*Truncated — full records contain 58 fields. See Output fields for the complete schema.*


**[Try SEEK Job Scraper - Australia & New Zealand Job Listings now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi)**


## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002 |

See the [actor on Apify](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) for current pricing.

---

## Related products by Black Falcon Data





- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal
- [Bilbasen Scraper](https://apify.com/blackfalcondata/bilbasen-scraper?fpr=1h3gvi) — Denmark's largest car marketplace


## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. [Sign up free](https://console.apify.com/sign-up?fpr=1h3gvi) — $5 credit included
2. Open the actor and paste your input
3. Click Start — results download as JSON, CSV, or Excel

Need more volume? [See pricing](https://apify.com/pricing?fpr=1h3gvi).

---


## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---
---

*Last updated: 2026 03*
