# React Email + OpenUI — Bundler Comparison

This repo demonstrates the same AI email generator app running across three different bundlers/frameworks using the same OpenUI packages:

- **`next/`** — Next.js (serves both the API and frontend)
- **`vite/`** — Vite + React (frontend only, proxies API to Next.js)
- **`cra/`** — Create React App / webpack (frontend only, proxies API to Next.js)

## Architecture

The Next.js server runs the `/api/chat` endpoint that streams responses from OpenAI. Both the Vite and CRA frontends are pure client-side apps that proxy `/api` requests to the Next.js server.

```
vite (port 30334)  ──┐
                     ├──► next (port 30333) ──► OpenAI
cra  (port 30335)  ──┘
```

## Prerequisites

- Node.js 18+
- pnpm
- `OPENAI_API_KEY` set in `next/.env.local`

```
# next/.env.local
OPENAI_API_KEY=sk-...
```

## Running

All three projects must be run from their own directories.

### Next.js (required — runs the API)

```bash
cd next
pnpm install
pnpm dev
```

Runs on **http://localhost:30333**

### Vite

```bash
cd vite
pnpm install
pnpm dev
```

Runs on **http://localhost:30334** — proxies `/api` to `http://localhost:30333`

### Create React App (webpack)

```bash
cd cra
pnpm install
pnpm start
```

Runs on **http://localhost:30335** — proxies `/api` to `http://localhost:30333`

## Packages used

| Package | Purpose |
|---|---|
| `@openuidev/react-headless` | `ChatProvider`, `useThread` — streaming chat state |
| `@openuidev/react-lang` | `Renderer` — parses and renders OpenUI Lang responses |
| `@openuidev/react-email` | `emailLibrary` — email component library for the renderer |
| `@react-email/render` | Renders React Email components to HTML string |
