---
name: linkedin-job-search-export
description: Scrape LinkedIn job search results across all pages and export a cleaned CSV with title, company, link, salary, requirements, and results page.
compatibility: opencode
metadata:
  dependencies: chrome-devtools-mcp
  output: csv
---

## What I do
- Extract job listings from LinkedIn search results using browser automation.
- Paginate deterministically by `start` query param in increments of 25.
- Capture `Job Title`, `Company`, `Link to post`, `Salary`, `High level requirements`, and `Results page`.
- Save output to `~/JobHunt/search-results/YYYY-MM-DD/search-results.csv`.
- Clean output by removing non-job entries and deduplicating by normalized post link.

## When to use me
Use this skill when a user is on a LinkedIn jobs search results page and wants a structured CSV export of all listings from the current query.

## Requirements
- Active LinkedIn session in the browser.
- A loaded LinkedIn jobs search results page.
- `chrome-existing` MCP tools available (chrome-devtools-mcp).

## Workflow
1. Confirm current page is a LinkedIn jobs search URL and read `totalResults` from page text if available.
2. Build page coverage using `start` offsets (`0, 25, 50, ...`) from the current query.
3. For each offset page, scrape cards from the jobs search results list container only.
4. Keep only postings whose link matches `/jobs/view/`; discard collections, alerts, recommendations, and promoted side modules that are not job posts.
5. Normalize extracted values (trim whitespace, collapse duplicate title text, remove trailing UI artifacts like "with verification").
6. Generate CSV rows with a consistent schema and `N/A` for missing fields.
7. Write CSV to dated output directory under `~/JobHunt/search-results/`.
8. Run a cleanup pass to deduplicate by normalized job link.
9. Report final file path, total results seen, page offsets attempted, unique listings exported, and any shortfall reason.

## Notes
- LinkedIn UI can intermittently expose non-job panel links while scraping; cleanup is required.
- Requirement text may be unavailable for some postings and should be recorded as `N/A`.
- LinkedIn can show a persistent or misleading `Next` control; offset-based pagination is preferred over relying on the `Next` button state alone.
- If `totalResults` is unavailable, continue until an offset yields no new `/jobs/view/` links.
