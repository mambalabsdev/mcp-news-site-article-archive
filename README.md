# News Site Article Archive MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-news-site-article-archive)](https://www.npmjs.com/package/@mambalabsdev/mcp-news-site-article-archive) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-news-site-article-archive)](https://github.com/mambalabsdev/mcp-news-site-article-archive/blob/main/LICENSE)

An MCP server for the Mamba Labs News Site Article Archive actor on Apify. Full article text and dates from named news sites across a past date range.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give news sites and a date range. The actor finds article addresses in the site sitemaps and the Internet Archive capture index, reads each page over plain HTTP, and returns one row per article with full text, published date, title, author, and section. A page gone from the site can be read from an Internet Archive copy. Paywalled and refused pages return a free row with no text.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-news-site-article-archive": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-news-site-article-archive"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `collect_news_site_articles` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Get every article this news site published in January 2016."
- "Collect 10 years of reports from these three news sites."
- "Pull the full text of this site's politics stories from last year."

## Tool

`collect_news_site_articles`. One tool. Example arguments:

```json
{
  "sites": [
    "techcrunch.com"
  ],
  "date_from": "2016-01-01",
  "date_to": "2016-01-31",
  "max_articles_per_site": 5
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `sites` | array | no | News sites by domain, for example techcrunch.com, up to 20. |
| `date_from` | string | no | The earliest published date, as YYYY-MM-DD. Default: 30 days before Date to. |
| `date_to` | string | no | The latest published date, as YYYY-MM-DD. Default: today. |
| `max_articles_per_site` | number | no | Articles to return from each site, up to 5,000. Default 100. |
| `url_contains` | array | no | Optional. Keep only addresses with any of these texts, for example /politics/. |
| `min_words` | number | no | Pages with fewer words are not counted as articles and are free. Default 80. |
| `use_archive_index` | boolean | no | yes (default): when the sitemaps run out, also find addresses in the Internet Archive capture index, which reaches years that sitemaps no longer list. no: sitemaps only. |
| `use_archive_copy` | boolean | no | yes (default): a page the site answers with 404, 410, 500, 502, or 504 is read from the Internet Archive, at the archived row price. Refused and paywalled pages are never read from an archive. no: gone pages return a free row. |
| `obey_robots` | boolean | no | yes (default): pages robots.txt disallows are skipped and counted. no: read them anyway. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `article-row`, one article read from the publisher's page | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `archived-article-row`, one article read from an Internet Archive copy, only for a page gone from the site | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.014 | $0.014 | $0.014 | $0.014 |

Refused, paywalled, gone, and out of range pages, summary rows, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/news-site-article-archive](https://apify.com/mambalabs/news-site-article-archive)

## License

MIT. See [LICENSE](LICENSE).
