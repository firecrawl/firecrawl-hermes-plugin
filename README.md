# Firecrawl plugin for Hermes Agent

> Generated from Firecrawl's plugin source. Do not edit this repository by hand; changes made here are overwritten by the next sync.

<img src="assets/logo.png" alt="Firecrawl" width="96" height="96">

Firecrawl is the context API for AI agents. This plugin connects [Hermes Agent](https://hermes-agent.nousresearch.com) to the hosted [Firecrawl MCP server](https://docs.firecrawl.dev/mcp-server/oauth) and adds Firecrawl skills, so Hermes can search the web, read pages, collect structured data, interact with live pages, crawl sites, run research and track changes. It is an [Agent Plugins](https://agent-plugins.org) 1.0.0 package: a manifest, an MCP connection and Markdown skills, with no local code or runtime dependencies.

Hermes already ships Firecrawl as a backend for its built-in `web_search` and `web_extract` tools. This plugin is separate from that backend and does not replace it: it adds the full Firecrawl tool set (crawl, map, interact, agent, monitors, parse, Alexandria data providers, people and company search, and developer, paper and US government search) and skills that route requests to the right tool. Both can use the same `FIRECRAWL_API_KEY`.

## What's included

- **`mcp.json`**: the hosted Firecrawl MCP server at `https://mcp.firecrawl.dev/v2/mcp-oauth`, as the `firecrawl` server
- **`skills/get-started`**: checks the connection, then gets you to a useful first result
- **`skills/firecrawl`**: routes each request to the right Firecrawl tool, with a reference per operation
- **`skills/firecrawl-deep-research`**: cited, multi-source investigations, comparisons, prospect discovery and enrichment
- **Workflow skills**: `brand-identity`, `business-contact-research`, `design`, `enrich-lead`, `fantasy-sports`, `find-leads`, `flights-and-hotels`, `job-postings`, `local-businesses`, `markets-and-filings`, `news-coverage`, `prices-and-plans`, `property-listings`, `read-docs`, `seo-audit` and `video-transcripts`

Requires Hermes Agent 0.22.2 or later, with its MCP support installed.

## Install

```bash
hermes plugins install firecrawl-plugin
```

Portable packages are installed disabled. Enable it in the profile you chat from:

```bash
hermes plugins enable firecrawl-plugin
```

To install straight from this repository instead of the catalog:

```bash
hermes plugins install firecrawl/firecrawl-hermes-plugin --no-enable
```

## Connect your Firecrawl account

Both methods use the same endpoint, `https://mcp.firecrawl.dev/v2/mcp-oauth`. Choose **one**, and use the same Hermes profile for installation, configuration, sign-in and chat.

### Browser sign-in (OAuth, recommended)

Hermes runs OAuth for servers declared in your profile's `config.yaml` with `auth: oauth`, and the portable plugin format has no field for it, so browser sign-in takes one native entry. It takes precedence over the plugin's server of the same name. Replace any existing `mcp_servers.firecrawl` entry as a whole and keep your other servers:

```yaml
mcp_servers:
  firecrawl:
    url: https://mcp.firecrawl.dev/v2/mcp-oauth
    auth: oauth
    strict_redirect_headers: true
    headers:
      X-Firecrawl-Origin: firecrawl-hermes-plugin
```

Then sign in from a terminal:

```bash
hermes mcp login firecrawl
```

Hermes stores and refreshes the tokens; no `FIRECRAWL_API_KEY` is needed. Do not add an `Authorization` header to this entry. Cron jobs and other unattended runs cannot complete a browser sign-in, so sign in first, or use an API key for unattended work.

### API key

Create a key at [firecrawl.dev/app/api-keys](https://www.firecrawl.dev/app/api-keys) and save it to the active profile:

```bash
hermes config set FIRECRAWL_API_KEY fc-your-key
```

With no native entry, the plugin's `mcp.json` sends the key as a bearer token. If you already use Firecrawl as Hermes's web backend, the same key works. Keep the key out of chat, URLs and this repository.

## Use

Ask for what you need in plain language, or start with the `get-started` skill. Hermes names the tools `mcp__firecrawl__<tool>`, for example `mcp__firecrawl__firecrawl_search` and `mcp__firecrawl__firecrawl_scrape`; the skills refer to them by the tool name alone (`firecrawl_search`), and Hermes loads them as `firecrawl-plugin:<skill>`.

- "Search for this week's announcements about Agent Plugins and summarize them with links."
- "Scrape https://docs.firecrawl.dev and list the SDKs it documents."
- "Map docs.example.com and crawl the /api section."
- "Find 20 Series A fintech companies in London with their websites."
- "Use firecrawl-deep-research to compare the three largest vector databases on pricing and limits."

## Capabilities

- **Search**: web, news and image search with query-relevant excerpts, plus matching Alexandria data providers
- **Scrape**: any URL to clean Markdown, JSON with a schema, a grounded answer or verbatim highlights
- **Alexandria**: discover catalogued data providers and execute them for structured records
- **People and companies**: search by title, employer, location or industry, and enrich known people or companies
- **Agent**: autonomous multi-source collection into structured data
- **Map and crawl**: list a site's URLs or collect a bounded section
- **Interact**: clicks, forms and navigation in a live browser session
- **Parse**: PDFs and documents, including local files through a hosted upload
- **Monitor**: scheduled change detection for pages and web searches
- **Developer, paper and government search**: repositories and docs, research papers with full text, and US law and regulation

## What this plugin does on your machine and the network

- Sends the queries, URLs, prompts and files you ask Firecrawl to process to `https://mcp.firecrawl.dev` over HTTPS, authenticated with your OAuth sign-in or API key. Tool calls use your Firecrawl account's credits; `firecrawl_credit_usage` reports usage.
- Sends `X-Firecrawl-Origin: firecrawl-hermes-plugin` so Firecrawl can attribute requests to this plugin. There is no separate telemetry.
- `firecrawl_parse` uploads a local file only when you ask it to parse that file.
- Runs no local code, shell commands, background processes or self-updater, and reads no credential files. Hermes owns the MCP connection, credentials and approvals.
- The hosted server's tools can change independently of this package; the live tool schema is the authority.

## Support

- Docs: [docs.firecrawl.dev](https://docs.firecrawl.dev)
- Issues: [github.com/firecrawl/firecrawl-mcp-server/issues](https://github.com/firecrawl/firecrawl-mcp-server/issues)
- Email: help@firecrawl.dev

## License

MIT
