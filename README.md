[Dockerhub Scraper](https://apify.com/automation-lab/dockerhub-scraper?fpr=data)

Scrape Docker Hub repositories from [hub.docker.com](https://hub.docker.com). Search by keyword and get image names, descriptions, star counts, pull counts, and official status.

## What does Docker Hub Scraper do?

Docker Hub Scraper uses the Docker Hub search API to find container image repositories. It extracts repository names, descriptions, star counts, pull counts, official image status, and automated build status. Search for specific technologies or browse popular images.

## Who is it for?

- 🐳 **DevOps engineers** — tracking popular Docker images, tags, and pull counts
- 🔒 **Security teams** — auditing container image usage and identifying outdated versions
- 📊 **Technology analysts** — measuring container adoption trends across the ecosystem
- 🏢 **Engineering managers** — evaluating which base images are most maintained and trusted
- 💻 **Platform engineers** — discovering and comparing container images for infrastructure

## Why scrape Docker Hub?

Docker Hub is the world's largest container image registry with millions of repositories. It's the primary source for understanding container image adoption and popularity.

Key reasons to scrape it:

- **Infrastructure research** — Find popular base images and tools for your stack
- **Adoption metrics** — Track pull counts to gauge technology adoption
- **Competitive analysis** — Monitor competing images in your technology space
- **Security research** — Identify widely-used images for vulnerability assessment
- **DevOps intelligence** — Track trends in containerized tools and services

## Use cases

- **DevOps engineers** finding the best base images for their infrastructure
- **Platform teams** researching container ecosystem options
- **Security teams** auditing widely-deployed container images
- **Technical writers** finding popular images for container tutorials
- **Cloud architects** evaluating technology stack options
- **Researchers** studying container adoption patterns

## How to scrape Docker Hub

1. Go to [Docker Hub Scraper](https://apify.com/automation-lab/dockerhub-scraper) on Apify Store
2. Enter one or more search keywords
3. Set result limits
4. Click **Start** and wait for results
5. Download data as JSON, CSV, or Excel

## Input parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `searchQueries` | string[] | (required) | Keywords to search for |
| `maxResultsPerSearch` | integer | `50` | Max repositories per keyword |
| `maxPages` | integer | `3` | Max pages (25 repos per page) |

### Input example

```
{
    "searchQueries": ["nginx", "redis"],
    "maxResultsPerSearch": 25,
    "maxPages": 1
}
```

## Output

Each repository in the dataset contains:

| Field | Type | Description |
| --- | --- | --- |
| `name` | string | Repository name |
| `description` | string | Short description |
| `stars` | number | Star count |
| `pulls` | number | Total pull count |
| `isOfficial` | boolean | Official Docker image |
| `isAutomated` | boolean | Automated build enabled |
| `dockerHubUrl` | string | Docker Hub page URL |
| `scrapedAt` | string | ISO timestamp of extraction |

### Output example

```
{
    "name": "nginx",
    "description": "Official build of Nginx.",
    "stars": 21196,
    "pulls": 12827491090,
    "isOfficial": true,
    "isAutomated": false,
    "dockerHubUrl": "https://hub.docker.com/_/nginx",
    "scrapedAt": "2026-03-03T03:56:31.123Z"
}
```

## How much does it cost to scrape Docker Hub?

Docker Hub Scraper uses pay-per-event pricing:

| Event | Price |
| --- | --- |
| Run started | $0.001 |
| Repository extracted | $0.001 per repo |

### Cost examples

| Scenario | Repos | Cost |
| --- | --- | --- |
| Quick search | 25 | $0.026 |
| Category survey | 100 | $0.101 |
| Large analysis | 250 | $0.251 |

Platform costs are negligible — typically under $0.001 per run.

## Using Docker Hub Scraper with the Apify API

### Node.js

```
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });

const run = await client.actor('automation-lab/dockerhub-scraper').call({
    searchQueries: ['nginx'],
    maxResultsPerSearch: 25,
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(`Found ${items.length} repositories`);
items.forEach(repo => {
    const official = repo.isOfficial ? '[Official]' : '';
    console.log(`${official} ${repo.name} (${repo.pulls.toLocaleString()} pulls, ${repo.stars} stars)`);
});
```

### Python

```
from apify_client import ApifyClient

client = ApifyClient('YOUR_API_TOKEN')

run = client.actor('automation-lab/dockerhub-scraper').call(run_input={
    'searchQueries': ['nginx'],
    'maxResultsPerSearch': 25,
})

dataset = client.dataset(run['defaultDatasetId']).list_items().items
print(f'Found {len(dataset)} repositories')
for repo in dataset:
    official = '[Official]' if repo['isOfficial'] else ''
    print(f"{official} {repo['name']} ({repo['pulls']:,} pulls, {repo['stars']} stars)")
```

## Integrations

Docker Hub Scraper works with all Apify integrations:

- **Scheduled runs** — Track image popularity trends over time
- **Webhooks** — Get notified when a scrape completes
- **API** — Trigger runs and fetch results programmatically
- **Google Sheets** — Export repository data to a spreadsheet
- **Slack** — Share popular images with your team

Connect to [Zapier](https://apify.com/integrations/zapier), [Make](https://apify.com/integrations/make), or [Google Sheets](https://apify.com/integrations/google-sheets) for automated workflows.

## Tips

- **Filter by `isOfficial: true`** to find Docker-maintained base images
- **Sort by pull count** to identify the most widely adopted images
- **Compare star counts** to gauge community engagement
- **Search for specific technologies** (e.g. "postgres", "node") to find relevant images
- **Track pull counts over time** with scheduled runs to spot adoption trends
- **Multiple keywords** let you compare adoption across technology categories

## Legality

Scraping publicly available data is generally legal according to the [US Court of Appeals ruling](https://en.wikipedia.org/wiki/HiQ_Labs_v._LinkedIn) (HiQ Labs v. LinkedIn). This actor only accesses publicly available information and does not require authentication. Always review and comply with the target website's Terms of Service before scraping. For personal data, ensure compliance with GDPR, CCPA, and other applicable privacy regulations.

## FAQ

**How many repositories can I search?**
Each page returns 25 repositories. With pagination, you can fetch hundreds per keyword.

**Does it include tag/version information?**
The search API returns repository-level metadata. For individual tags and versions, you'd need to query the tag-specific endpoints.

**Are private repositories included?**
No — only public repositories appear in Docker Hub search results.

**What do pull counts represent?**
Pull counts reflect the total number of times an image has been pulled (downloaded) from Docker Hub across all time.

**How often are pull counts updated?**
Pull counts are updated in near real-time as images are downloaded.

## Use with Claude AI (MCP)

This actor is available as a tool in Claude AI through the Model Context Protocol (MCP). Add it to Claude Desktop, Cursor, Windsurf, or any MCP-compatible client.

### Setup for Claude Code

```
$claude mcp add --transport http apify "https://mcp.apify.com?tools=automation-lab/dockerhub-scraper"
```

### Setup for Claude Desktop, Cursor, or VS Code

Add this to your MCP config file:

```
{
    "mcpServers": {
        "apify": {
            "url": "https://mcp.apify.com?tools=automation-lab/dockerhub-scraper"
        }
    }
}
```

### Example prompts

- "Search Docker Hub for Node.js images and show me the top 10 by pull count"
- "Find all official database images on Docker Hub and compare their popularity"
- "What are the most widely used Python base images on Docker Hub right now?"

Learn more in the [Apify MCP documentation](https://docs.apify.com/platform/integrations/mcp).

### cURL

```
curl "https://api.apify.com/v2/acts/automation-lab~dockerhub-scraper/run-sync-get-dataset-items?token=YOUR_API_TOKEN" \
  -X POST -H "Content-Type: application/json" \
  -d '{"searchQueries": ["nginx"], "maxResultsPerSearch": 25}'
```

**I only see community images, not official ones.**
Official images appear in search results alongside community images. Filter by `isOfficial: true` in post-processing to isolate them.

**Pull counts seem extremely high — are they accurate?**
Yes. Popular official images like `nginx`, `node`, and `python` have billions of pulls accumulated over years of usage across CI/CD pipelines, development environments, and production systems worldwide.

## Other package registry scrapers

- [npm Scraper](https://apify.com/automation-lab/npm-scraper) — Scrape package data from the npm registry
- [PyPI Scraper](https://apify.com/automation-lab/pypi-scraper) — Scrape package data from the Python Package Index
- [Crates Scraper](https://apify.com/automation-lab/crates-scraper) — Scrape crate data from crates.io (Rust)
- [Homebrew Scraper](https://apify.com/automation-lab/homebrew-scraper) — Scrape formula data from Homebrew
- [Pub.dev Scraper](https://apify.com/automation-lab/pubdev-scraper) — Scrape package data from pub.dev (Dart/Flutter)