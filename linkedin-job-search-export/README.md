# linkedin-job-search-export

This skill standardizes a LinkedIn job-search scraping workflow that exports all results to a cleaned CSV.

## Dependency

- `chrome-devtools-mcp` (used through `chrome-existing` tools for navigation, pagination, and extraction)

## Prerequisites

- Chrome must be started in remote debugging mode so MCP can attach to the existing browser session.
- You must already be logged into LinkedIn in that browser session.
- You must already have a LinkedIn jobs search results page open with your desired filters.

## Output

- Primary output path: `~/JobHunt/search-results/YYYY-MM-DD/search-results.csv`
- Columns:
  - `Job Title`
  - `Company`
  - `Link to post`
  - `Salary`
  - `High level requirements`
  - `Results page`

## Pagination strategy

- Do not rely only on the LinkedIn `Next` button state.
- Use deterministic pagination by `start` query param in steps of 25 (`start=0,25,50,...`).
- Stop when `start >= totalResults` (if detectable), or when a new page yields no new `/jobs/view/` links.

## Extraction rules

- Scrape from the jobs search results list container only.
- Accept only links that match `/jobs/view/`.
- Exclude non-job modules (recommended collections, alerts, side panel links, and other non-search cards).
- Normalize text by trimming whitespace, collapsing duplicated title text, and removing UI suffixes such as `with verification`.

## Usage expectations

- User should keep the same browser tab/session active while the scrape runs.
- Skill should report:
  - final CSV path
  - total results detected
  - page offsets attempted
  - unique listing count exported
  - shortfall reason if exported count is lower than expected

## Development process

- Blog post: https://www.maxcroy.com/writing/job-hunt-part-1-network/
