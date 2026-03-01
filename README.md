# ETHRPC.monitor — Decentralized RPC Health Dashboard

An open-source dashboard that monitors **Ethereum Mainnet RPC providers** in real time. It pings a curated list of public RPC endpoints, measures latency and availability, and surfaces metrics so you can compare centralized vs decentralized providers and choose resilient endpoints.

**Live preview:** [https://decentralized-rpc-hub.netlify.app](https://decentralized-rpc-hub.netlify.app)

---

## What the app does

### Live RPC health checks

- **Pings 24+ public Ethereum RPC endpoints** every 30 seconds via `eth_blockNumber` JSON-RPC calls.
- **Measures response latency** (ms) and marks each provider as **healthy** (&lt;1000 ms), **degraded**, or **down**.
- **Aggregates stats** across all providers: total/healthy count, average latency, current block height.

No backend required: health checks run in the browser. Data is **live from Ethereum Mainnet** and refreshes automatically.

### Dashboard sections

| Section | Description |
|--------|-------------|
| **Metric cards** | Six summary cards: Total Nodes (estimated), Avg Latency, Avg Uptime, Block Height, % Decentralized providers, and Healthy vs Total providers. |
| **RPC Provider Benchmark table** | Sortable, filterable table of every provider with: name, type (light-client / p2p / decentralized / centralized), status, latency, uptime, censorship score, node count, region, block height. Each row includes a **sparkline** of recent latency. |
| **Latency over time (24h)** | Area chart comparing latency trends for representative providers (sample/demo data until enough live history is collected). |
| **Censorship Resistance Ranking** | Providers ranked by **censorship resistance score** (0–100). Scores are static metadata indicating how resistant each provider is considered to censorship. |
| **Geographic Distribution** | Bar chart of estimated node distribution by region (North America, Europe, Asia Pacific, etc.). Uses reference data to illustrate decentralization. |

### Provider types and filters

- **Types**: Light Client, P2P, Decentralized, Centralized.
- **Filters**: By status (healthy / degraded / down), by type, and by region. Sort by name, latency, uptime, censorship score, or node count.

### Optional wallet connect

- Header includes a **Connect Wallet** button that uses the **MetaMask** provider (`window.ethereum`) to request accounts. If MetaMask isn’t installed, it links to the MetaMask download page. No RPC data depends on the wallet.

---

## Tech stack

- **React 18** + **TypeScript**
- **Vite** (dev server and build)
- **TanStack Query (React Query)** for fetching and refetching RPC health (30s interval, 10s stale time)
- **Recharts** for latency-over-time and sparklines
- **Tailwind CSS** + **shadcn/ui** (Radix-based components)
- **Supabase** client is present (e.g. for future auth or persistence); the dashboard itself does not require Supabase to run

---

## Run locally

**Requirements:** Node.js and npm (e.g. [install Node via nvm](https://github.com/nvm-sh/nvm#installing-and-updating)).

```bash
# Install dependencies
npm install

# Start the dev server (hot reload)
npm run dev
```

The app is served at **http://localhost:8080**.

**Production build and preview:**

```bash
npm run build
npm run preview
```

Output is in `dist/`. Deploy that folder to any static host (Vercel, Netlify, GitHub Pages, etc.).

---

## Environment (optional)

If you use Supabase elsewhere in the app, set in `.env`:

- `VITE_SUPABASE_URL`
- `VITE_SUPABASE_PUBLISHABLE_KEY`
- `VITE_SUPABASE_PROJECT_ID`

The RPC health dashboard works without these; it only calls the configured public RPC endpoints from the browser.

---

## Project structure (overview)

| Path | Purpose |
|------|--------|
| `src/pages/Index.tsx` | Main dashboard page: layout, metrics, table, charts, geo. |
| `src/hooks/useRpcHealth.ts` | Defines RPC endpoint list and `useRpcHealth()` — fetches health for all endpoints and returns providers + stats. |
| `src/data/mockRpcData.ts` | Enriches live data (censorship scores, node estimates, latency history), and holds fallback/sample data for latency-over-time and geo. |
| `src/components/dashboard/` | `DashboardHeader`, `MetricCard`, `ProviderTable`, `LatencyChart`, `CensorshipScoreCard`, `GeoDistribution`, `StatusBadge`, `SparklineChart`. |
| `src/App.tsx` | Root: React Query, router, tooltips, toasters; single route `/` → `Index`. |

---

## Scripts

| Command | Description |
|--------|-------------|
| `npm run dev` | Start Vite dev server at http://localhost:8080 |
| `npm run build` | Production build → `dist/` |
| `npm run preview` | Serve `dist/` locally |
| `npm run lint` | Run ESLint |
| `npm run test` | Run Vitest once |
| `npm run test:watch` | Run Vitest in watch mode |

---

## Data and privacy

- All RPC requests are **public `eth_blockNumber`** calls from the user’s browser to the listed RPC URLs.
- No RPC request includes wallet addresses or private data.
- Censorship scores and node counts are **curated metadata** in the repo, not measured in real time.

---

## License and attribution

ETHRPC.monitor — open-source decentralized RPC health dashboard. Data refreshes every 30s from Ethereum Mainnet.
