# EdgeEver

[简体中文](README.zh-CN.md) | English

> **EdgeEver: A self-hosted, Cloudflare-native Evernote alternative.**

EdgeEver is an open-source, self-hosted, Cloudflare-native notes workspace. It keeps the classic Evernote-style three-pane experience while providing a clear data model, REST API, OpenAPI schema, and Remote MCP endpoint.

With Cloudflare's free quotas, a personal deployment can run at nearly zero long-term cost.

## Why EdgeEver

Many long-time Evernote users only need a reliable, open, responsive personal knowledge base. But modern commercial notes apps are often heavier than necessary, harder to migrate away from, and increasingly shaped by subscription and add-on features.

EdgeEver fills that gap: familiar notes interaction, open data, API access, MCP support, and self-hosted deployment that is practical for individuals.

## Online Demo

- Demo: [https://demo.edgeever.org](https://demo.edgeever.org)

The public demo resets daily and restores sample notes. Do not store private content there.

## Deployment

Fork this repository to your own GitHub account first, then deploy from your fork. When EdgeEver ships updates, you can use GitHub's **Sync fork** button and redeploy to Cloudflare.

### Deploy with an AI Agent

Copy this prompt into your AI coding assistant, such as Claude Code, Codex, Antigravity, Cursor, or Trae:

```text
Please fork the EdgeEver repository first: https://github.com/tianma-if/edgeever

After the fork is ready, use the forked repository to install and deploy EdgeEver to Cloudflare, and configure automatic upstream sync for the fork so future product updates can be pulled in.
```

Agents should follow [AI Agent Cloudflare Deployment](docs/agent-deploy-cloudflare.md).

### Manual Deployment

1. Fork the official repository:

   ```text
   https://github.com/tianma-if/edgeever
   ```

2. Clone your fork:

   ```sh
   git clone <your fork repository URL>
   cd edgeever
   ```

3. Deploy with the helper commands:

   ```sh
   cp .env.local.example .env.local
   bun install
   EDGE_EVER_PASSWORD='<your password>' bun run deploy:setup
   bun run deploy:doctor
   bun run deploy
   ```

If you prefer creating Cloudflare resources manually:

```sh
cp .env.local.example .env.local
bun install
bunx wrangler d1 create edgeever
bunx wrangler r2 bucket create edgeever-resources
bun run auth:hash -- <your password>
bun run deploy
```

Put the returned D1 `database_id` and password hash into your local `.env.local`.

## Updating

If you deployed from a fork:

1. Open your EdgeEver fork on GitHub.
2. Click **Sync fork** to pull the latest official code.
3. Redeploy locally:

   ```sh
   git pull
   bun install
   bun run deploy:doctor
   bun run deploy
   ```

Syncing the fork only updates your GitHub repository. You still need to redeploy for the Cloudflare instance to change.

## Features

- Nearly zero-cost personal hosting on Cloudflare D1 + R2 free quotas.
- Open data: notes are stored in Cloudflare D1, based on standard SQLite, and can be read through REST API, MCP, and CLI.
- AI Agent friendly: built-in MCP support lets tools such as Codex, Claude Code, and Antigravity read and organize notes with authorization.
- Desktop and mobile support, including browser access and PWA installation.
- Three-pane layout: notebook tree, note list, and main editor.
- Unlimited nested notebooks.
- Rich text editing.
- Note version history for reviewing previous content changes.
- Local browser-side image compression before upload, often reducing screenshots and large photos by about 50%-90%.
- Batch note merging.
- Batch note moving, notebook drag sorting, and hierarchy editing.
- Offline drafts and local sync queue for existing notes.
- Single-user login with PBKDF2-SHA256 password hashing.

## PWA Installation

EdgeEver can be installed as a PWA on desktop or mobile home screens. On desktop, open the site in Chrome or Edge and use the install icon in the address bar. On Android, open it in Chrome, use the three-dot menu, and choose **Add to Home screen** or **Install**. Avoid installing from embedded browsers such as WeChat.

## Tech Stack

- Bun workspace monorepo with Web, API, official site, and shared type package.
- Official site: Astro static site in `apps/site`, deployable to Cloudflare Pages.
- Frontend: Vite, React, React Router, TanStack Query, Tailwind CSS, shadcn/ui, and Radix UI.
- Editor: TipTap / ProseMirror with Markdown support; PWA uses vite-plugin-pwa, Workbox, and Dexie.
- Backend: Cloudflare Workers, Hono, Zod, D1, and R2, with REST API, OpenAPI, and Remote MCP.

## Quick Start

Install dependencies:

```sh
bun install
```

Apply local D1 migrations:

```sh
bun run db:migrate:local
```

Start local development:

```sh
bun run dev
```

Checks:

```sh
bun run typecheck
bun run build
```

## Project Structure

```text
apps/web          Vite + React frontend, PWA, offline drafts, and sync queue
apps/api          Cloudflare Worker + Hono API, OpenAPI, MCP endpoint
apps/site         Astro official website, deployable independently
packages/shared   Shared types, Zod schemas, TipTap / Markdown conversion
scripts           Wrangler wrapper, password hash, CLI, MCP stdio bridge, Evernote ENEX import
migrations        D1 database migrations
docs              OpenAPI schema, migration guides, and deployment docs
wrangler.toml     Cloudflare Workers, Assets, D1, R2 configuration
```

## Content Formats

EdgeEver stores note content in three forms:

```text
content_json      TipTap/ProseMirror document, the editor source of truth
content_markdown  API, Agent, import, and export format
content_text      Search, summary, and indexing text
```

## API

OpenAPI schema:

```text
https://your-domain/api/openapi.json
```

Repository file: [docs/openapi.json](docs/openapi.json).

## MCP

Create an API token in **Profile** -> **MCP settings**, then copy either the token or full MCP configuration into your AI Agent so it can install the MCP server and read or organize notes with permission.

## Image Compression

Image compression happens in the Web client before upload and is controlled by the **Compress note images** setting. When enabled, PNG, JPEG, WebP, and AVIF files are converted to WebP when beneficial, with the longest edge limited to `2560px`. If compression does not reduce size, the original file is kept.

EdgeEver avoids Worker-side image processing to reduce compute and image-processing quota usage. REST API and MCP upload paths store the file content provided by the client without additional server-side compression.

## Community and Feedback

- Bugs, feature requests, and deployment issues: [GitHub Issues](https://github.com/tianma-if/edgeever/issues)
- WeChat: `m1245207870` (please mention EdgeEver)
