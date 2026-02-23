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

## Usage expectations

- User should keep the same browser tab/session active while the scrape runs.
- Skill should continue pagination until no `Next` page exists.
