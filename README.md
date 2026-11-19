# TSX networks

Official chain configurations, RPC endpoints, and verified contract addresses for **TSX Horizon**, **TSX Bastion**, and **TSX Testnet**.

The live source of truth is [tsx.network/networks.json](https://tsx.network/networks.json) and the contracts table on [tsx.network](https://tsx.network). This repository is a curated snapshot of those values, checked against the public RPCs and explorers.

## Developer quickstart

| Network | Chain ID | Hex | Access | RPC | Explorer |
| --- | ---: | --- | --- | --- | --- |
| [TSX Horizon](https://scan.tsx.network/horizon) | `455` | `0x1c7` | Anyone can deploy and use it | `https://horizon.tsx.network/rpc` | [scan.tsx.network/horizon](https://scan.tsx.network/horizon) |
| [TSX Bastion](https://scan.tsx.network/bastion) | `454` | `0x1c6` | Approved builders can deploy; anyone can use it | `https://bastion.tsx.network/rpc` | [scan.tsx.network/bastion](https://scan.tsx.network/bastion) |
| [TSX Testnet](https://scan.tsx.network/testnet) | `445` | `0x1bd` | Open, like Horizon. Coins have no value | `https://testnet.tsx.network/rpc` | [scan.tsx.network/testnet](https://scan.tsx.network/testnet) |

The coin is **TSX** on every chain, with **18 decimals**. Check the chain ID your wallet shows against this list before you send anything.

Blocks are made on demand. A TSX chain makes a block only when there is a transaction to include, so a long time since the last block means the chain is idle, not down.

Start on Testnet, move to Horizon when you are ready, and write to [builders@tsx.network](mailto:builders@tsx.network) if your product belongs on Bastion.

### Add a network in MetaMask

In MetaMask, open **Settings → Networks → Add network → Add a network manually**, or send a `wallet_addEthereumChain` request. The `wallet` object in each file under [`chains/`](./chains/) is the EIP-3085 payload.

| Field | Horizon | Bastion | Testnet |
| --- | --- | --- | --- |
| Network name | TSX Horizon | TSX Bastion | TSX Testnet |
| RPC URL | `https://horizon.tsx.network/rpc` | `https://bastion.tsx.network/rpc` | `https://testnet.tsx.network/rpc` |
| Chain ID | `455` | `454` | `445` |
| Currency symbol | `TSX` | `TSX` | `TSX` |
| Block explorer | `https://scan.tsx.network/horizon` | `https://scan.tsx.network/bastion` | `https://scan.tsx.network/testnet` |

One-click buttons and the Testnet faucet (10 TSX per address every 24 hours) are on [tsx.network](https://tsx.network). Test coins have no value and cannot be sold.

Example `wallet_addEthereumChain` request for Testnet:

```js
await window.ethereum.request({
  method: 'wallet_addEthereumChain',
  params: [
    {
      chainId: '0x1bd',
      chainName: 'TSX Testnet',
      nativeCurrency: { name: 'TSX', symbol: 'TSX', decimals: 18 },
      rpcUrls: ['https://testnet.tsx.network/rpc'],
      blockExplorerUrls: ['https://scan.tsx.network/testnet'],
      iconUrls: ['https://tsx.network/brand/icon.png'],
    },
  ],
});
```

### Point your tools at Testnet

```js
import { createPublicClient, defineChain, http } from 'viem';

export const tsxTestnet = defineChain({
  id: 445,
  name: 'TSX Testnet',
  nativeCurrency: { name: 'TSX', symbol: 'TSX', decimals: 18 },
  rpcUrls: { default: { http: ['https://testnet.tsx.network/rpc'] } },
  blockExplorers: {
    default: { name: 'TSX Explorer', url: 'https://scan.tsx.network/testnet' },
  },
  testnet: true,
});

const client = createPublicClient({ chain: tsxTestnet, transport: http() });
console.log(await client.getBlockNumber());
```

```bash
cast block-number --rpc-url https://testnet.tsx.network/rpc
```

Swap the RPC and chain ID for Horizon (`455`, `https://horizon.tsx.network/rpc`) or Bastion (`454`, `https://bastion.tsx.network/rpc`).

## Repository layout

| File | What it is |
| --- | --- |
| [`chains/horizon-455.json`](./chains/horizon-455.json) | Horizon chain config (EIP-3085 `wallet` object and chainlist-compatible fields) |
| [`chains/bastion-454.json`](./chains/bastion-454.json) | Bastion chain config |
| [`chains/testnet-445.json`](./chains/testnet-445.json) | Testnet chain config |
| [`contracts.json`](./contracts.json) | Verified contract addresses only |
| [tsx.network/networks.json](https://tsx.network/networks.json) | Live official network list |

Horizon and Bastion configs also include the Avalanche `blockchainId` used for interchain messages, taken from the official sale and bridge configuration on tsx.network. A Testnet `blockchainId` is not published there, so it is not listed here.

## Verified contracts

These addresses were published on [tsx.network](https://tsx.network) and confirmed as source-verified on the relevant explorer on 19 November 2026 (Europe/Warsaw). **"Verified" means the explorer has matched the published source code to the bytecode at that address. It is not a security review.** The sale and bridge contracts have not been audited.

| Contract | Chain | Address | Explorer |
| --- | --- | --- | --- |
| TSX sale | Avalanche C-Chain | `0x58a6f17E7427CB4FfFa57aE5381Dc2592ca48062` | [Snowtrace](https://snowtrace.io/address/0x58a6f17E7427CB4FfFa57aE5381Dc2592ca48062) |
| Sale pool | Horizon | `0xf65B8cA243C338daD78652D44C920921A957DC45` | [Explorer](https://scan.tsx.network/horizon/address/0xf65B8cA243C338daD78652D44C920921A957DC45) |
| Sale pool | Bastion | `0xf65B8cA243C338daD78652D44C920921A957DC45` | [Explorer](https://scan.tsx.network/bastion/address/0xf65B8cA243C338daD78652D44C920921A957DC45) |
| Bridge pool | Horizon | `0x178d7c04cF2D2166DcD345d319D2fEb90c9b0629` | [Explorer](https://scan.tsx.network/horizon/address/0x178d7c04cF2D2166DcD345d319D2fEb90c9b0629) |
| Bridge pool | Bastion | `0x178d7c04cF2D2166DcD345d319D2fEb90c9b0629` | [Explorer](https://scan.tsx.network/bastion/address/0x178d7c04cF2D2166DcD345d319D2fEb90c9b0629) |
| Multicall3 | Horizon, Bastion, Testnet | `0xcA11bde05977b3631167028862bE2a173976CA11` | [Horizon](https://scan.tsx.network/horizon/address/0xcA11bde05977b3631167028862bE2a173976CA11) · [Bastion](https://scan.tsx.network/bastion/address/0xcA11bde05977b3631167028862bE2a173976CA11) · [Testnet](https://scan.tsx.network/testnet/address/0xcA11bde05977b3631167028862bE2a173976CA11) |
| CreateX | Horizon, Bastion, Testnet | `0xba5Ed099633D3B313e4D5F7bdc1305d3c28ba5Ed` | [Horizon](https://scan.tsx.network/horizon/address/0xba5Ed099633D3B313e4D5F7bdc1305d3c28ba5Ed) · [Bastion](https://scan.tsx.network/bastion/address/0xba5Ed099633D3B313e4D5F7bdc1305d3c28ba5Ed) · [Testnet](https://scan.tsx.network/testnet/address/0xba5Ed099633D3B313e4D5F7bdc1305d3c28ba5Ed) |
| Avalanche messenger | Horizon, Bastion, and Avalanche C-Chain | `0x253b2784c75e510dD0fF1da844684a1aC0aa5fcf` | [Horizon](https://scan.tsx.network/horizon/address/0x253b2784c75e510dD0fF1da844684a1aC0aa5fcf) · [Bastion](https://scan.tsx.network/bastion/address/0x253b2784c75e510dD0fF1da844684a1aC0aa5fcf) · [Snowtrace](https://snowtrace.io/address/0x253b2784c75e510dD0fF1da844684a1aC0aa5fcf) |

Sale payment tokens on Avalanche C-Chain, as published on tsx.network: USDC `0xB97EF9Ef8734C71904D8002F8b6Bc66Dd9c48a6E` and USDT `0x9702230A8Ea53601f5cD2dc00fDBc13d4dF4A8c7`.

The following official site entries are **not** in the verified list:

- **CREATE2 proxy** `0x4e59b44847b379578588920cA78FbF26c0B4956C` is published on tsx.network and is present on Horizon, Bastion, and Testnet (69 bytes of bytecode), but explorers cannot verify it because it is hand-written Yul. The site's note: check it yourself with `solc 0.5.8 --strict-assembly --optimize` on its source.
- The Avalanche messenger is **not on Testnet**, per tsx.network and the Testnet explorer.
- No sale pool or bridge pool is published for Testnet.

Oracle addresses and any other unpublished contracts are omitted until they appear on tsx.network and an explorer confirms their source.

## What TSX is, and what it is not

TSX is a **utility token**: the coin that pays for transactions and smart contracts on TSX chains. It is a prepayment for use of Horizon and Bastion, issued by 3S Game Studio OÜ. Buying TSX is one way: it cannot be redeemed for the money or stablecoins you paid, and nobody promises it will hold its price.

TSX is not an investment, a share, a claim on anyone's profit or assets, a stablecoin, or e-money. It is not offered to US persons. It is not affiliated with the Toronto Stock Exchange. Nothing in this repository is financial or investment advice.

Under Regulation (EU) 2023/1114 on markets in crypto-assets (MiCA), TSX is a utility token: a crypto-asset other than an asset-referenced token or an e-money token, intended only to give access to a service supplied by its issuer. Crypto-assets can lose their value in part or in full and may not be liquid.

## Contact

- Builders and Bastion access: [builders@tsx.network](mailto:builders@tsx.network)
- Security reports: [security@tsx.network](mailto:security@tsx.network) (see [SECURITY.md](https://github.com/TSX-Blockchain/.github/blob/main/SECURITY.md))
- General: [hello@tsx.network](mailto:hello@tsx.network)
- Website: [tsx.network](https://tsx.network)
- X: [@tsx_blockchain](https://x.com/tsx_blockchain)
- Discord: [discord.gg/BuErakdcw](https://discord.gg/BuErakdcw)

---

By the TSX team.
