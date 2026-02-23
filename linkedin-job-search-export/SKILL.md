---
name: linkedin-job-search-export
description: Scrape LinkedIn job search results across all pages and export a cleaned CSV with title, company, link, salary, and high-level requirements.
compatibility: opencode
metadata:
  dependencies: chrome-devtools-mcp
  output: csv
---

## What I do
- Extract job listings from LinkedIn search results using browser automation.
- Paginate through all available result pages until the end.
- Capture `Job Title`, `Company`, `Link to post`, `Salary`, and `High level requirements`.
- Save output to `~/JobHunt/search-results/YYYY-MM-DD/search-results.csv`.
- Clean output by removing non-job entries and deduplicating by post link.

## When to use me
Use this skill when a user is on a LinkedIn jobs search results page and wants a structured CSV export of all listings from the current query.

## Requirements
- Active LinkedIn session in the browser.
- A loaded LinkedIn jobs search results page.
- `chrome-existing` MCP tools available (chrome-devtools-mcp).

## Workflow
1. Confirm current page context and available pagination state.
2. Scrape visible result cards on the current page.
3. Click `Next` and repeat scraping until there is no next page.
4. Generate CSV rows with a consistent schema and `N/A` for missing fields.
5. Write CSV to dated output directory under `~/JobHunt/search-results/`.
6. Run a cleanup pass to remove non-job rows and deduplicate by job link.
7. Report final file path, page coverage, and listing count.

## Notes
- LinkedIn UI can intermittently expose non-job panel links while scraping; cleanup is required.
- Requirement text may be unavailable for some postings and should be recorded as `N/A`.
