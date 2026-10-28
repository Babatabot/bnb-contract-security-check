# BNB Contract Security Check

## Accept one contract

- Ask for one contract address and the chain if the user knows it.

- Treat a missing chain as unknown and never assume mainnet or testnet.

- Keep the address exactly as the user gave it and copy it into every query.

- Ask for the source code only after the platform checks are done, because the source audit is a separate step.

## Determine the chain first

- Call `eth_getCode` on BSC mainnet and on BSC testnet for the same address.

- Record the chain id, the code length and the balance for each chain.

- Use mainnet results when only mainnet has code and testnet results when only testnet has code.

- Stop and ask the user when both chains have code or when neither chain has code.

## Run the API lines first

- Open the GoPlus API for chain 56 and then for chain 97 when the first chain returns no data.

- Record the full JSON result and keep the original field values for tax, mint, blacklist, proxy, pause, honeypot, owner, holders and LP holders.

- Treat an empty GoPlus result as no data and never write it as a pass.

- Open the Honeypot.is API for the same chain and record whether the simulation succeeded, whether the token is a honeypot, and the buy tax, sell tax and transfer tax.

- Compare the two API lines and keep any disagreement for the cross check step.

## Run the live browser platforms

- Open Token Sniffer, De.Fi Scanner, Quick Intel and DappBay Risk Scanner in real browser windows.

- Use `browser.browse` with one new window per site and give each window the full address and the token name when known.

- Wait for JavaScript to finish loading and read the rendered result instead of the page shell.

- Expect Cloudflare checks on Token Sniffer and Quick Intel and let the browser task work through them without logging in or paying.

- Record the score, the item list and the original wording for each platform.

- Treat a blank page or a shell page as no data and never fill the gap from memory.

## Verify the contract on chain

- Read the BscScan token page for the correct chain and record whether the source is verified, the compiler version, the optimisation setting and the licence.

- Call `name`, `symbol`, `decimals` and `totalSupply` when the contract is a token.

- Call `owner` and read the owner storage slot when the contract has an owner function.

- Read the EIP-1967 implementation slot, admin slot and beacon slot and record whether a proxy is present.

- List the function selectors from the bytecode and match them against the verified ABI.

- Record every selector you cannot identify and never guess its function name.

## Cross check the platforms

- Build one merged list of checks from the seven platforms and mark each platform as covered, not covered, partial, republished or unverified.

- Read [platform checks](references/platform-checks.md) for the verified item counts and field lists.

- Read [conflict rules](references/conflict-rules.md) before you resolve any disagreement.

- Write a disagreement as a side by side conflict and never pick one side without evidence.

- Keep the holder concentration conclusion on the platforms that agree and keep the minority figure visible.

- Keep the transfer pause conclusion on the verified ABI and the current mode and owner state.

- Explain LP burn and LP lock as two different measures and never use one to cancel the other.

## Deliver the report

- Deliver one PDF report as the core output and give a short chat summary in the same message.

- Put the chain, the token name, the total supply and the overall verdict on the first page.

- Put a platform by dimension table next and then one page per platform.

- Put every unresolved conflict on its own page with the original values from each side.

- Mark every unknown item as unknown and never write it as no risk.

- Create the PDF with `artifact.create_file` and update it with `artifact.edit` when a later browser run adds data.

- Name the file after the token or the contract and keep it under the platform goal when the check supports that goal.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Websites

- [GoPlus Token Security](https://console.gopluslabs.io/token-security) for the structured API fields on permissions, tax, holders and LP.

- [Honeypot.is](https://honeypot.is/) for a live buy and sell simulation on BSC.

- [Token Sniffer](https://tokensniffer.com/) for a 0 to 100 score, similar contracts, holder distribution and liquidity.

- [Quick Intel](https://app.quickintel.io/scanner) for hidden owner, hidden address, external contract risk and LP burn checks.

- [De.Fi Scanner](https://de.fi/scanner) for 62 code and permission checks in guest mode.

- [CertiK Token Scan](https://skynet.certik.com/tools/token-scan/bsc) for 23 itemised checks on BSC.

- [DappBay Risk Scanner](https://dappbay.bnbchain.org/risk-scanner) for a republished view of GoPlus, CertiK and HashDit results.

## Alternatives for better results

- Run the four live browser platforms when text fetching returns no data, because Cloudflare and JavaScript rendering block the text route.

- Use DappBay to confirm which CertiK item caused an Attention when the CertiK text alone does not say.

- Ask the user for the source code when the bytecode has unidentified selectors, because platform checks alone cannot prove mint, tax change, blacklist or pause powers.

- Re-run all seven platforms after a mainnet deployment, because testnet results do not carry over to mainnet.
