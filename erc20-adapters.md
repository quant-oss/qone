# ERC-20 Adapter Registry — QVTX

Canonical ERC-20 token data has **one source of truth per component**. This document maps each major
DeFi/wallet component to its authoritative token registry endpoint.

## Source-of-Truth Map

| Component | Source URL | Format | Chains |
|-----------|------------|--------|--------|
| **QVTX** | `quant-oss/qone` (this repo) | Token List v2 | 7+ |
| MetaMask | `MetaMask/contract-metadata` index.json | `{address: {name,symbol,decimals}}` | ETH only |
| Uniswap | `https://tokens.uniswap.org` | Token List v2 | Multi |
| SushiSwap | `https://token-list.sushi.com` | Token List v2 | Multi |
| Trust Wallet | `trustwallet/assets` blockchains/{chain}/tokenlist.json | Token List v2 | Per-chain |
| 1inch | `https://tokens.1inch.io/v1.2/{chainId}` | Dict by address | Multi |
| CoinGecko | `https://tokens.coingecko.com/uniswap/all.json` | Token List v2 | ETH |
| Coinbase Wallet | `coinbase/coinbase-wallet-sdk` walletlink-default-tokens.json | Array | Multi |
| Li.Fi | `https://li.quest/v1/tokens` | Dict by chainId | Multi |
| Paraswap | `https://apiv5.paraswap.io/tokens/{chainId}` | Tokens array | Multi |
| OpenOcean | `https://open-api.openocean.finance/v3/{chain}/tokenList` | Data array | Multi |
| KyberSwap | `https://ks-setting.kyberswap.com/api/v1/tokens` | data.tokens array | Multi |
| Etherscan | `https://api.etherscan.io/api?module=token&action=tokeninfo` | Per-contract | ETH |
| DefiLlama | `https://coins.llama.fi/prices/current/{chain}:{addr}` | Coins dict | Multi |

## ByteID Fold Key

Every QVTX token identity folds to a ByteID via:
```
key = f"{chainId}:{address.lower()}:{symbol}:{decimals}"
byteid_v3_pair(key.encode(), store=True)  # → (sense, shadow)
```

See `qusd.tokenlist.json` → `extensions.byteid_key` for all entries.

## Token Lists

- `qvtx.tokenlist.json` — QONE + older QUSD entries with full ByteID backing proofs
- `qusd.tokenlist.json` — wallet-active QUSD deployments (7 chains, sourced from background.js)

## MCP Server

`erc20-adapters` MCP is registered in Claude Code settings at
`~/.claude/mcp-servers/erc20-adapters/erc20_adapters_mcp.py`.

Tools: `erc20_qvtx_tokens`, `erc20_lookup_token`, `erc20_byteid_key`, plus one tool per registry source.

## Canonical Token List URL (for DefiLlama registration)

```
https://raw.githubusercontent.com/quant-oss/qone/main/qusd.tokenlist.json
```

Submit this URL to: `github.com/DefiLlama/defillama-app` → `src/utils/tokenlists.ts`