# QVTX One (QONE)

Official repository for **QVTX One**, issued by QUANTVESTRIX, INC.

Website: **https://qone.qusd.us** · X: **[@QvtxQusd](https://x.com/qvtxqusd)** · Full contract registry: **https://qusdscan.us/contracts**

| | |
|---|---|
| Name | QVTX One |
| Symbol | `QONE` |
| Decimals | `18` |
| Total supply | `500,000,000,000` |
| Chains | Ethereum, Polygon, BNB Chain, Base, Arbitrum One |

## Contract addresses

Always take the address from this file or from https://qone.qusd.us — never from a message
someone sends you. A token imported under the wrong address is a different token.

| Network | Chain ID | Address |
|---|---|---|
| Ethereum | 1 | `0xba2aAAf685f58156b6420D06FA169B61ED53B945` |
| Polygon | 137 | `0xC88BaD1688bD1725F02f3CF6e5657c1155a4D12c` |
| BNB Chain | 56 | `0x4A9463EFf350d90927A45b9Bd39F59eBa24c7969` |
| Base | 8453 | `0x4618091C333819e913de6632Bc26D60503E4bAF6` |
| Arbitrum One | 42161 | `0x3ECcc6E1A19C867E066eD44E4aC539979C1023cf` |

## Verified source

The contract source is verified on **Sourcify** with an **exact match on both the creation
bytecode and the deployed runtime bytecode** on all five chains. That is the strongest
verification result available, and it is independent of us — check any of these yourself:

- Ethereum — https://sourcify.dev/server/v2/contract/1/0xba2aAAf685f58156b6420D06FA169B61ED53B945
- Polygon — https://sourcify.dev/server/v2/contract/137/0xC88BaD1688bD1725F02f3CF6e5657c1155a4D12c
- BNB Chain — https://sourcify.dev/server/v2/contract/56/0x4A9463EFf350d90927A45b9Bd39F59eBa24c7969
- Base — https://sourcify.dev/server/v2/contract/8453/0x4618091C333819e913de6632Bc26D60503E4bAF6
- Arbitrum One — https://sourcify.dev/server/v2/contract/42161/0x3ECcc6E1A19C867E066eD44E4aC539979C1023cf

Build: `solc 0.8.24+commit.e11b9ed9`, optimizer enabled at 200 runs, `viaIR`, EVM version
`london`, with the `QVTXFold` library linked. Identical settings on every chain.

Sourcify records are read by Blockscout, Otterscan, Remix and Foundry.

### A note on Etherscan

QONE is **not** currently verified on Etherscan, and the reason is on their side rather than
in the source. The same standard-JSON input verifies `QVTXOneCollateral`, `QVTXFold`,
`QVTXFoldRegistry`, `QVTXParVault` and `QVTXOneFactory` on Etherscan across all five chains —
25 of our 30 deployments carry an Etherscan record. QONE is the only contract with a large
number of library link sites (20) and the only one near the EIP-170 size ceiling
(24,475 bytes of runtime against a 24,576 limit), and it is refused on every chain with the
same bytecode-mismatch message, including when the exact bundle Sourcify certifies as an
exact match is submitted unchanged. A support request is open with them.

We state this plainly rather than leave it to be discovered.

## The systems behind it

QONE is not a token issued onto someone else's infrastructure. The estate runs its own
chains, its own multi-chain rail, its own price oracle and its own banking core. Every
figure below was read live on 2026-10-22.

| System | What it is | Live reading |
|---|---|---|
| **QUSD Oracle** | Our own oracle, exposing the standard Chainlink `AggregatorV3` surface so any integrator reading a conventional feed can read it unmodified. Same address on four chains: `0xc234af77d0884ffb31a1a604f212bd5d6c75a4e5` | **1.00000000** at 8 decimals on BNB Chain, Polygon, Base and Arbitrum — 2,076 bytes of code on each |
| **QVTX RPC rail** | A multi-chain read rail that answers only when independent upstreams agree byte for byte; disagreement returns an error rather than a silently chosen answer | **14 chains** served |
| **QVTX chains** | Three chains the estate runs itself, no third party in the path | 42000 — block 1,445,232<br>20232 — block 1,712,779<br>420018 — block 309,478 |
| **Metatron** | The banking core, with a read-only viewer surface | responding |
| **STC** | ISO 20022 sender-side API — pacs.008 delivery and pacs.002 receipt issuance, with a compliance gate on every message | 32 assets, 30 reservations, 29 redemptions |
| **Era** | The treasury book of record across the estate's accounts and containers | 29 accounts |
| **Fold rail** | Content is stored by folding it to a ByteID the content can be *derived* from, with an independent shadow strand. Identity is the fold of the content, not a label attached to it | every published artefact is folded and recovers byte-exact |

The oracle is QUANTVESTRIX's own. It is third-party *readable*, not third-party *derived*,
and is published as such rather than presented as independent valuation.

## ISO 20022 carriage

The mint identifier travels on bank rails without truncation. A full ByteID is 38
characters, which exceeds ISO 20022's `Max35Text`; the contract stores a 28-character form
that fits, and the full identifier is reproduced from it exactly — `isoByteIdOf(tokenIdOf(...))`
returns `AGA20d7a05f8a8a2db02d12X7392` for ByteID `QVTX-AGA-20d7a05f-8a8a2db0-2d12-X-7392`.

A pacs.008 carrying the ByteID in `SplmtryData` has been accepted by a live receiver
returning **ACSC** — AcceptedSettlementCompleted.

## Token list

Standard token-list format, covering QONE, QUSD and vQONE across all five chains:

```
https://qusdscan.us/qvtx.tokenlist.json
```

## Price oracle

QUSD is priced by an on-chain oracle at `0xc234af77d0884ffb31a1a604f212bd5d6c75a4e5`,
deployed at the same address on BNB Chain, Polygon, Base and Arbitrum. It exposes the
standard Chainlink `AggregatorV3` surface and returned `1.00000000` (8 decimals) on all four
chains at the last reading.

This is QUANTVESTRIX's own oracle. It is third-party *readable*, not third-party *derived*,
and is published as such.

## Supply

Total supply is fixed at 500,000,000,000 QONE with 18 decimals, under an immutable maximum
set at deployment. The supply cap is a one-way ratchet: it can only ever be raised toward
that immutable maximum, never lowered and never exceeded.

## Security

We will never DM you first, never ask for a seed phrase, and never ask you to send funds to
"verify" a wallet. Anyone doing so is not us.

---

Nothing in this repository is an offer, a solicitation, or investment advice.
