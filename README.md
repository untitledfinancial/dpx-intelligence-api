# DPX Intelligence API

[![Live](https://img.shields.io/badge/live-intelligence.untitledfinancial.com-brightgreen)](https://intelligence.untitledfinancial.com)
[![License: BSD-3-Clause](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

**Built by Victoria Lee Case** — founder, creator, and sole technical builder of DPX and Untitled_ LuxPerpetua Technologies, Inc.

---

**Live:** `https://intelligence.untitledfinancial.com`  
**License:** BSD 3-Clause — © 2024-2026 Victoria Case / Untitled_ LuxPerpetua Technologies, Inc.

---

## What it is

The DPX Intelligence API is a standalone pay-per-call data product. It exposes the signal output of the [DPX Stability Oracle](https://github.com/untitledfinancial/dpx-stability-oracle) as individual intelligence endpoints — each one a focused briefing on a specific domain: macro stress, climate, supply chain, energy, ESG, earth systems, and systemic risk.

No API key. No subscription. Pay per call in USDC on Base mainnet via [x402](https://x402.org).

---

## Part of the DPX product suite

| Service | Domain | Role |
|---------|--------|------|
| [Stability Oracle](https://github.com/untitledfinancial/dpx-stability-oracle) | `stability.untitledfinancial.com` | 10-layer signal backbone |
| [ESG Oracle](https://github.com/untitledfinancial/dpx-esg-oracle) | `esg.untitledfinancial.com` | ESG scores → settlement fees |
| [Compliance Oracle](https://github.com/untitledfinancial/dpx-compliance-public) | `compliance.untitledfinancial.com` | VoP · FATF R16 · AML |
| **Intelligence API** | `intelligence.untitledfinancial.com` | Pay-per-call intelligence endpoints |
| [DPX MCP Server](https://github.com/untitledfinancial/dpx-mcp) | `mcp.untitledfinancial.com` | 83 MCP tools for AI agents |

---

## Paid endpoints

**Base URL:** `https://intelligence.untitledfinancial.com`

Payment via x402 — USDC on Base mainnet (chainId 8453). No wallet setup required for agents using the [DPX MCP server](https://github.com/untitledfinancial/dpx-mcp).

| Endpoint | Price | Cache | What it returns |
|----------|-------|-------|-----------------|
| `GET /intelligence/macro-stress` | $0.15 | 1h | Credit regime classification — spread, volatility, and lending signals with 2–6 week lead time for FX and commodity moves |
| `GET /intelligence/climate` | $0.25 | 24h | Precipitation anomalies across 10 agricultural zones vs historical baseline — commodity exposure and food inflation signals |
| `GET /intelligence/climate-pulse` | $0.25 | 6h | Near real-time climate anomaly pulse — active weather extremes, drought indices, and flood risk by region |
| `GET /intelligence/supply-chain` | $0.25 | 6h | Global supply chain pressure indices and critical waterway monitoring — goods inflation lead signal |
| `GET /intelligence/energy-transition` | $0.25 | 24h | Renewable share, grid carbon intensity, fossil demand curve |
| `GET /intelligence/esg/:address` | $0.25 | 6h | Entity-level ESG score from multi-source regulatory, environmental, and corporate compliance data |
| `GET /intelligence/earth-systems` | $0.50 | 48h | Planetary health dashboard — atmospheric concentrations, temperature, sea ice vs pre-industrial baseline, proximity to 9 climate tipping points |
| `GET /intelligence/instability` | $0.50 | 1h | Composite instability signal — cross-tier regime detection when macro, geopolitical, and climate signals compound |
| `GET /intelligence/cascade` | $0.75 | 30m | Full cascade risk model — shock propagation across tiers, amplification loops, forward scenario tree |
| `GET /intelligence/commodity` | $0.25 | — | Commodity price signal |
| `GET /intelligence/sovereign-debt` | $0.25 | — | Sovereign debt stress signal |
| `GET /intelligence/water-risk` | $0.25 | — | Water scarcity/risk signal |
| `GET /intelligence/mycelium` | $0.50 | — | Mycelium Network Oracle — crisis formation from network topology |
| `GET /intelligence/currency-stress` | $0.25 | — | Currency stress signal |
| `GET /intelligence/biodiversity` | $0.25 | — | Biodiversity risk signal |
| `POST /intelligence/butterfly` | $0.50 | — | Butterfly-effect cascade model |
| `GET /intelligence/tectonic` | $0.50 | — | Slow-moving structural break detection, 6–18 month horizon |
| `POST /intelligence/aftershock` | $0.50 | — | Secondary shock modeling across financial corridors |
| `POST /intelligence/contagion` | $0.50 | — | Shock propagation through the financial network |
| `GET /intelligence/resonance` | $0.50 | — | Amplifying/compounding macro signal detection |
| `GET /intelligence/gender-risk` | $0.50 | — | GBV risk and female economic opportunity scoring |
| `GET /intelligence/shipping-stress` | $0.25 | — | Shipping and logistics stress signal |
| `GET /intelligence/fx-settlement` | $0.25 | — | FX settlement risk signal |
| `POST /intelligence/composite` | $1.00 | — | Composite cross-tier synthesis |
| `POST /intelligence/synthesis` | $1.00 | — | Full AI synthesis across all signal tiers |
| `GET /intelligence/political-risk` | $0.50 | — | Political risk signal |
| `GET /intelligence/transition-risk` | $0.75 | — | Climate transition risk signal |
| `GET /intelligence/sfdr-dashboard` | $0.25 | — | SFDR PAI indicator dashboard (requires `?address=`) |
| `GET /intelligence/48h-call` | $0.50 | — | 48-hour macro call |
| `GET /intelligence/financed-emissions` | $0.25 | — | PCAF-aligned financed-emissions estimate (requires `?address=`) |
| `GET /intelligence/taxonomy-alignment` | $0.25 | — | EU Taxonomy alignment (requires `?address=`) |
| `POST /intelligence/tnfd-report` | $1.00 | — | TNFD LEAP nature-related risk report |

32 paid endpoints total. Some (marked `—` above) aren't yet in the live discovery manifest at `GET /` — see [ENDPOINT_REGISTRY.md](https://docs.untitledfinancial.com) for the always-current list.

---

## Free oracle feeds

`GET /oracle-feed` and `GET /oracle-feed/:signal` — lightweight numeric signals for on-chain consumption. No payment required.

Returns scaled integers (no floats) suitable for Chainlink Functions, API3 dAPI, and any protocol that needs a clean integer on-chain.

```bash
curl https://intelligence.untitledfinancial.com/oracle-feed
# → { feed: "stability-score", value: 8100, scale: 100, signal: "STABLE" }

curl https://intelligence.untitledfinancial.com/oracle-feed/earth-health-index
curl https://intelligence.untitledfinancial.com/oracle-feed/atmospheric-co2
```

---

## x402 payment flow

```typescript
import { withPaymentInterceptor } from 'x402-fetch';

const fetchWithPayment = withPaymentInterceptor(fetch, wallet);

const data = await (
  await fetchWithPayment('https://intelligence.untitledfinancial.com/intelligence/macro-stress')
).json();
// → { stressIndex: 42, regime: "LATE_CYCLE", leadSignals: { fxImplication: "USD_STRENGTH_RISK", ... } }
```

1. Client sends request — no payment header
2. Server returns `402 Payment Required` with payment details
3. Client pays in USDC on Base mainnet
4. Server verifies on-chain and returns the intelligence response

See [x402.org](https://x402.org) for wallet setup and client libraries.

---

## Using via MCP

AI agents can access all intelligence endpoints without managing wallets directly via the [DPX MCP server](https://github.com/untitledfinancial/dpx-mcp):

```
intelligence.briefing  → MPP-gated macro briefing
oracle.status          → full 10-layer oracle output including AI synthesis
```

Install once, available in Claude Desktop, Cursor, and any MCP-compatible host:

```bash
npx -y @untitledfinancial/dpx-mcp
```

---

## Compliance

GENIUS Act (US) · MiCA (EU) · EU SFDR/CSRD · Basel III · FATF Travel Rule

---

## Links

- **Live API**: [intelligence.untitledfinancial.com](https://intelligence.untitledfinancial.com)
- **Stability Oracle**: [stability.untitledfinancial.com](https://stability.untitledfinancial.com)
- **MCP server**: [mcp.untitledfinancial.com](https://mcp.untitledfinancial.com)
- **npm**: [@untitledfinancial/dpx-mcp](https://www.npmjs.com/package/@untitledfinancial/dpx-mcp)
- **Docs**: [docs.untitledfinancial.com](https://docs.untitledfinancial.com)

---

## License

BSD 3-Clause. Copyright (c) 2024-2026 Victoria Case / [Untitled_ LuxPerpetua Technologies, Inc.](https://untitledfinancial.com)
