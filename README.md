# BNB Contract Security Check

Check a BNB Chain contract across 7 security platforms and deliver a PDF report.

BNB 合约安全检查，7 个平台深度检测并出报告。

## What it does

Give the skill one contract address. It checks the contract on GoPlus, Honeypot.is, Token Sniffer, Quick Intel, De.Fi Scanner, CertiK Token Scan and DappBay Risk Scanner, verifies the contract on chain with BscScan and public RPC, cross checks the platforms, and delivers a PDF report.

## Platforms

- GoPlus Token Security, API first
- Honeypot.is, live buy and sell simulation
- Token Sniffer, 0 to 100 score, similar contracts, holder and liquidity analysis
- Quick Intel, 27 checks including hidden owner, hidden address and LP burn
- De.Fi Scanner, 62 checks, 22 detectors plus 40 general detectors
- CertiK Token Scan, 23 itemised checks
- DappBay Risk Scanner, republished GoPlus, CertiK and HashDit results

## How to use

Copy this folder into your Muse custom skills directory:

```text
~/workspace/custom-skills/bnb_contract_security_check/
```

Then ask Muse to check a contract, for example:

```text
Check this BNB contract: 0x924fa68a0fc644485b8df8abfa0a41c2e7744444
```

Or search for the skill by name `bnb_contract_security_check`.

## How it works

1. Determine the chain first. Call `eth_getCode` on BSC mainnet and on BSC testnet. Never assume mainnet.
2. Run the API lines first. GoPlus and Honeypot.is. An empty result is no data, never a pass.
3. Run the four live browser platforms. Token Sniffer, De.Fi, Quick Intel and DappBay need a real browser because Cloudflare and JavaScript rendering block text fetching.
4. Verify on chain. BscScan source verification, owner, proxy slots and function selectors. List unidentified selectors, never guess them.
5. Cross check. Build one merged list, mark each platform as covered, not covered, partial, republished or unverified. Write disagreements side by side.
6. Deliver a PDF report with a platform by dimension table, one page per platform and one page for unresolved conflicts.

## Files

- `SKILL.md`, the skill instructions
- `skill.yaml`, the skill descriptor
- `references/platform-checks.md`, verified check items for all 7 platforms
- `references/conflict-rules.md`, how to handle the conflicts found in real checks

## Rules the skill follows

- No data is never a pass.
- A disagreement is never resolved by picking one side without evidence.
- LP burn and LP lock are two different measures.
- Scores from different platforms are never converted into one total score.
- Testnet results do not carry over to mainnet.

## License

MIT. See `LICENSE`.
