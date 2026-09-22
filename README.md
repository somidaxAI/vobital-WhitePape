# Vobital Protocol

> On-chain DeFi aggregator powered by SOMIDAX $SMDX — swap, pool, and earn across EVM & Solana ecosystems.

[![License: MIT](https://img.shields.io/badge/License-MIT-cyan.svg)](LICENSE)
[![Node](https://img.shields.io/badge/node-22-green.svg)](https://nodejs.org)
[![pnpm](https://img.shields.io/badge/pnpm-10-orange.svg)](https://pnpm.io)
[![Cloudflare Pages](https://img.shields.io/badge/Deployed-Cloudflare%20Pages-F38020?logo=cloudflare)](https://vobital.com)
[![Version](https://img.shields.io/badge/version-0.6.1-blueviolet)](package.json)

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Live App](#live-app)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [API Reference](#api-reference)
- [SDK](#sdk)
- [Desktop App — Windows .exe](#desktop-app--windows-exe)
- [Deployment — Cloudflare Pages](#deployment--cloudflare-pages)
- [Token — SOMIDAX $SMDX](#token--somidax-smdx)
- [Smart Contracts](#smart-contracts)
- [Security & Audits](#security--audits)
- [Contributing](#contributing)
- [Roadmap](#roadmap)
- [License](#license)

---

## Overview

Vobital Protocol is a non-custodial, multi-chain DEX aggregator that routes swaps through the **VobitalRouter** first — giving $SMDX holders the tightest spreads and lowest fees — before falling back to external liquidity (Uniswap, PancakeSwap, Raydium).

```
User Input
    │
    ▼
VobitalRouter (priority — 0.001 spread, lowest fee)
    │  no liquidity?
    ▼
External DEXes (Uniswap v3 · PancakeSwap · Raydium)
    │
    ▼
Best-price route → On-chain execution
```

Supports **EVM chains** (Ethereum mainnet, BNB Chain) and **Solana** in one unified interface.

---

## Features

| Feature | Description |
|---|---|
| **VobitalRouter** | Proprietary smart routing — Vobital pools scanned first, 0.001 spread priority |
| **Multi-chain** | EVM (Ethereum, BNB Chain) + Solana in one interface |
| **Token Disperse** | Batch-send ETH or ERC-20 to multiple addresses in one transaction |
| **SMDX Vault** | Stake $SMDX for yield — pool creation and liquidity provision |
| **Live Price Ticker** | Multi-source: CoinGecko → Binance → CryptoCompare (auto-fallback cascade) |
| **DeFi News Feed** | CryptoCompare · CoinDesk · Decrypt — switchable source tabs |
| **TradingView Charts** | Embedded live candlestick charts per trading pair |
| **On-chain Activity** | Real-time wallet transaction history via Etherscan / BscScan |
| **Token Listing** | Self-serve tiers: Free → Verified → Premium → VIP |
| **EVM Wallet Picker** | Full connector modal — MetaMask, WalletConnect, Coinbase Wallet, and more |
| **Solana Wallet Picker** | Phantom, Solflare, Backpack, and more |
| **Light / Dark theme** | Full theme toggle with persisted preference |
| **Mobile responsive** | Works on all screen sizes — PWA-ready |
| **Desktop app** | Native Electron wrapper for Windows (.exe), macOS (.dmg), Linux (.AppImage) |

---

## Live App

| Environment | URL |
|---|---|
| Production | [vobital.com](https://vobital.com) |
| GitHub | [github.com/vobitalDifi/vobital-protocol](https://github.com/vobitalDifi/vobital-protocol) |

---

## Getting Started

### Prerequisites

- [Node.js 22+](https://nodejs.org)
- [pnpm 10+](https://pnpm.io/installation) — `npm install -g pnpm`

### Clone & Install

```bash
git clone https://github.com/vobitalDifi/vobital-protocol.git
cd vobital-protocol
pnpm install
```

### Run Development Server

```bash
pnpm dev
# → http://localhost:5173
```

### Build for Production (Web)

```bash
pnpm build
# output → dist/
```

### Preview Production Build Locally

```bash
pnpm preview
```

### Build Desktop App

```bash
pnpm build:win    # Windows .exe NSIS installer
pnpm build:mac    # macOS .dmg  (requires macOS runner)
pnpm build:linux  # Linux .AppImage
```

See [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) for CI / GitHub Actions build setup.

---

## Project Structure

```
vobital-protocol/
├── public/
│   ├── _redirects          # Cloudflare Pages — SPA fallback (/* /index.html 200)
│   └── _headers            # Cloudflare Pages — security headers + asset caching
├── electron/
│   ├── main.js             # Electron main process
│   └── preload.js          # Context bridge (security sandbox)
├── docs/
│   ├── GETTING_STARTED.md  # Detailed setup guide
│   ├── API.md              # REST API reference
│   ├── SDK.md              # @vobital/sdk usage
│   ├── DEPLOYMENT.md       # Cloudflare Pages + GitHub Actions
│   └── CONTRIBUTING.md     # PR guidelines + bug reporting
├── src/
│   ├── app/routes.ts       # React Router route table
│   ├── components/
│   │   ├── Layout.tsx      # App shell: nav, ticker, footer
│   │   └── SolanaWalletPicker.tsx
│   ├── config/
│   │   ├── wagmi.ts        # EVM wallet config (wagmi v2 + viem v2)
│   │   └── solana.ts       # Solana wallet adapter config
│   ├── contracts.ts        # Deployed contract addresses
│   ├── hooks/
│   │   ├── useTickerPrices.ts       # Multi-source price feed (cascade)
│   │   ├── useVobitalAggregator.ts  # VobitalRouter scan + quote
│   │   ├── useWalletActivity.ts     # On-chain tx history
│   │   ├── useDisperse.ts           # Batch token send
│   │   └── useEcosystem.ts          # EVM ↔ Solana toggle
│   ├── pages/
│   │   ├── SwapPage.tsx       # Swap, disperse, news, chart
│   │   ├── PoolPage.tsx       # SMDX Vault & staking
│   │   ├── ListingPage.tsx    # Token listing tiers
│   │   ├── ContractsPage.tsx
│   │   ├── DocsPage.tsx
│   │   ├── ApiReferencePage.tsx
│   │   ├── PrivacyPage.tsx
│   │   ├── TermsPage.tsx
│   │   ├── AuditPage.tsx
│   │   └── BountyPage.tsx
│   ├── index.css           # Tailwind v4 + CSS design tokens
│   └── main.tsx            # React 19 entrypoint
├── index.html
├── vite.config.ts
├── package.json
└── tsconfig.json
```

---

## API Reference

Full reference: [docs/API.md](docs/API.md)

The mock API runs automatically with `pnpm dev` — no separate server needed. In production, replace with real backend or serverless functions on Cloudflare Workers.

### Base URL

```
Development:  http://localhost:5173/api/v1
Production:   https://vobital.com/api/v1  (Cloudflare Worker — coming Q4 2026)
```

### `GET /api/v1/quote`

Get a swap quote with safety check.

**Parameters**

| Param | Type | Required | Description |
|---|---|---|---|
| `network` | `evm` \| `solana` | Yes | Target blockchain |
| `from` | string | Yes | Input token symbol |
| `to` | string | Yes | Output token symbol |
| `amount` | string | Yes | Input amount (human-readable) |
| `slippage` | number | No | Slippage tolerance % (default `0.5`) |

**Example**

```bash
curl "http://localhost:5173/api/v1/quote?network=evm&from=ETH&to=SMDX&amount=1.0"
```

**Response**

```json
{
  "protocol": "VOBITAL DEX Aggregator",
  "network": "evm",
  "route": ["ETH", "SMDX"],
  "estimatedOutput": "124937.500000",
  "priceImpact": "0.05%",
  "gasEstimated": "145000",
  "minAmountOut": "123688125000000000000000",
  "targetPool": "0x0000000000000000000000000000000000000000",
  "callData": "0x",
  "safetyGuard": {
    "status": "PASSED",
    "message": "Tokens verified safe against honeypots and high-tax exploits."
  },
  "timestamp": 1726589412000
}
```

---

## SDK

Full reference: [docs/SDK.md](docs/SDK.md)

The `@vobital/sdk` npm package is in development (target: Q4 2026). The API below is a preview of the planned interface.

### Installation (preview)

```bash
npm install @vobital/sdk
# or
pnpm add @vobital/sdk
```

### Usage

```typescript
import { VobitalClient } from '@vobital/sdk';

// Initialise
const client = new VobitalClient({
  network: 'ethereum',          // 'ethereum' | 'bsc' | 'solana'
  rpcUrl: 'https://...',        // optional — uses public RPC by default
});

// Get a quote
const quote = await client.getQuote({
  from: 'ETH',
  to: 'SMDX',
  amount: '1.0',                // human-readable, not wei
  slippage: 0.5,                // percent
});

console.log(quote.estimatedOutput); // "124937.500000"
console.log(quote.priceImpact);     // "0.05%"

// Execute swap (requires connected signer)
const txHash = await client.executeSwap(quote, signer);
console.log('tx:', txHash);

// Get live price
const price = await client.getPrice('SMDX'); // → 0.042

// Disperse tokens
const result = await client.disperse({
  token: 'ETH',                 // 'ETH' for native, or ERC-20 address
  recipients: [
    { address: '0xAbc...', amount: '0.1' },
    { address: '0xDef...', amount: '0.05' },
  ],
  signer,
});
```

### React hooks (coming with SDK)

```typescript
import { useVobitalQuote, useVobitalPrice } from '@vobital/sdk/react';

function SwapWidget() {
  const { quote, loading } = useVobitalQuote({ from: 'ETH', to: 'SMDX', amount: '1' });
  const { price } = useVobitalPrice('SMDX');
  // ...
}
```

---

## Desktop App — Windows .exe

Vobital ships as a native desktop app via [Electron](https://electronjs.org).

### Build locally

```bash
# Install all deps (includes electron-builder)
pnpm install

# Windows — produces dist-electron/Vobital Protocol Setup 0.6.1.exe
pnpm build:win

# macOS — produces dist-electron/Vobital Protocol-0.6.1.dmg
pnpm build:mac

# Linux — produces dist-electron/Vobital Protocol-0.6.1.AppImage
pnpm build:linux
```

### Build via GitHub Actions (recommended)

See [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) — the included workflow builds Windows, macOS, and Linux installers automatically on every tagged release and uploads them as GitHub Release assets.

### Architecture

```
electron/main.js        — Creates BrowserWindow, loads dist/index.html
electron/preload.js     — Context bridge: exposes safe node APIs to renderer
src/ (React app)        — Unchanged — runs identically in browser or Electron
```

---

## Deployment — Cloudflare Pages

### One-click setup

1. Push repo to GitHub
2. [Cloudflare Dashboard](https://dash.cloudflare.com) → Workers & Pages → Create → Pages → **Connect to Git**
3. Select `vobitalDifi/vobital-protocol`

### Build configuration

| Setting | Value |
|---|---|
| Framework preset | (none) |
| Build command | `pnpm install && pnpm run build` |
| Build output directory | `dist` |
| Node.js version | `22` |

### Custom domain

Pages → Custom domains → Add `vobital.com` and `www.vobital.com`.
Cloudflare handles SSL, CDN caching, and DDoS protection automatically.

### How SPA routing works

`public/_redirects` contains `/* /index.html 200` — Cloudflare serves the React bundle for every path so React Router handles `/swap`, `/pool`, `/bounty` etc. without server-side rendering.

`public/_headers` applies security headers and 1-year immutable caching on hashed JS/CSS assets at the CDN edge.

---

## Token — SOMIDAX $SMDX

| Property | Detail |
|---|---|
| Symbol | $SMDX |
| Networks | Ethereum · BNB Chain |
| ETH contract | `0x7e8539d1E5Cb91d63e46b8E188403B3f262a949b` |
| BNB contract | `0xEA8c5B9c537f3ebBcc8F2df0573F2d084E9e2BDb` |
| Launch price | $0.042 |
| Utility | Swap fee discounts · Vault yield · Governance voting · Token listing fees |

---

## Smart Contracts

| Contract | Network | Address | Status |
|---|---|---|---|
| SMDX Token | Ethereum | `0x7e8539d1E5Cb91d63e46b8E188403B3f262a949b` | Live |
| SMDX Token | BNB Chain | `0xEA8c5B9c537f3ebBcc8F2df0573F2d084E9e2BDb` | Live |
| VobitalRouter | Ethereum | — | Q4 2026 |
| VobitalRouter | BNB Chain | — | Q4 2026 |
| SMDX Vault | Ethereum | — | Q4 2026 |

All contracts will be open-source, audited, and verified on Etherscan / BscScan before mainnet launch.

---

## Security & Audits

- Independent audit scheduled Q4 2026 — [Audit page](https://vobital.com/audit)
- Bug Bounty active — up to **$50,000 SMDX** — [Bug Bounty](https://vobital.com/bounty)
- 3-of-5 multisig with 48-hour timelock on all admin calls
- Non-custodial — private keys never leave your device
- Static analysis: Slither + Mythril on every PR

**Responsible disclosure:** report vulnerabilities privately via Discord `#bug-bounty` — do not disclose publicly before patch.

---

## Contributing

See [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md).

```bash
git checkout -b feat/your-feature
pnpm dev
# make changes, write tests
git commit -m "feat: short description"
git push origin feat/your-feature
# open a Pull Request against main
```

Commit convention: `feat:` `fix:` `docs:` `refactor:` `chore:`

---

## Roadmap

| Version | Target | Status | Highlights |
|---|---|---|---|
| v0.1 | Q2 2026 | ✅ | Core swap interface |
| v0.2 | Q2 2026 | ✅ | Solana integration |
| v0.3 | Q3 2026 | ✅ | VobitalRouter priority routing |
| v0.4 | Q3 2026 | ✅ | Token disperse + listing tiers |
| v0.5 | Q3 2026 | ✅ | EVM wallet picker modal |
| v0.6 | Q3 2026 | ✅ | Multi-source feeds · Legal pages · Cloudflare deploy |
| v0.7 | Q4 2026 | 🔄 | VobitalRouter mainnet deployment |
| v0.8 | Q4 2026 | 📅 | Smart contract audit |
| v0.9 | Q4 2026 | 📅 | `@vobital/sdk` npm package |
| v1.0 | Q1 2027 | 📅 | Desktop app (Win / Mac / Linux) |
| v1.1 | Q2 2027 | 📅 | iOS & Android mobile apps |

---

## License

MIT © 2026 Vobital Protocol / SOMIDAX

