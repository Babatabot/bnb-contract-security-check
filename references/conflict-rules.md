# 口径冲突处理规则

本文件记录 2026 年 10 月 27 日至 28 日实测中真实出现过的冲突。遇到同类冲突时按以下规则处理，不要替用户选边。

## 先判链

五个测试网合约的地址在 BSC 主网全部为空地址，GoPlus 主网 API 返回空结果，CertiK 明确 404。币安人生的地址只在 BSC 主网有代码，测试网为空。

先用主网和测试网 RPC 的 eth_getCode 各查一次，确认链归属后再选平台口径。不要把主网扫描器无数据写成地址错误，也不要把测试网合约按主网写。

## 转账能否暂停

GoPlus 的 transfer_pausable 为 1，CertiK、HashDit、Quick Intel、De.Fi 四家都说未发现暂停机制。

已验证源码中没有 pause 函数，但有所有者门控的 setMode 和三档模式，分别是正常模式 0、转账受控模式 1、转账受限模式 2。当前模式存储槽为 0，即正常模式。所有者为零地址，所以 setMode 目前无人可调。

报告中要并列三方原话，结论写代码有 setMode 开关、不是 pause 函数、当前正常模式且无人可调。不要只取一方写死可暂停或不可暂停。

## LP 是烧毁还是锁定

GoPlus 说主池 LP 99.9976% 在销毁地址且标记锁定。CertiK 说 LP Locked Ratio 为 0%。Token Sniffer、Quick Intel、DappBay 说 99.99% 或 100% 已烧毁或锁定。

这是两种口径。烧毁进死地址不等于锁仓合约锁定。LP 实际已不可取回，但不属于锁仓合约锁定。报告中要分开解释，不要用一方否定另一方。

## 持仓集中度

GoPlus、CertiK、DappBay 三家一致，前 10 持有人约 81%，前 2 名约 65.76%。Token Sniffer 的持币分析却写前 10 持有人占 17.4%。

两者严重冲突，且无法调和。报告中要并列标注为未解决冲突，风险结论暂以三家一致的 81% 为准，17.4% 不覆盖、不删除。

## 创建者是谁

GoPlus、CertiK、DappBay 三家一致，创建者是 0x8463902e1fe88f63b79c11014dc221eb37fa6533。De.Fi 显示的创建者是 0xf10b 开头的另一个地址。

两者对不上。报告中要并列存疑，不要写死创建者是谁。

## CertiK 的 Attention 是哪一项

文本抓取无法唯一确定。真浏览器打开 DappBay 内 CertiK 标签后可以确认，币安人生那 1 个 Attention 就是大户占比 24.93%。

以后遇到 CertiK 只报数量不报项目时，用 DappBay 内 CertiK 标签或真浏览器打开 CertiK 原页确认，不要猜。

## De.Fi 分数内部不一致

De.Fi 主页面显示 DeFi Score 97%，页面底部嵌入卡片却显示 100%。两者是同一页面的两个位置，报告中要并列标注，不要只取一个。

## 评分不可直接比较

Token Sniffer 100 分、De.Fi Safety 100 分、DeFi Score 97%、CertiK 0 Alerts 1 Attention 22 Passed、Quick Intel 0 警告，各家量尺不同。报告中只写各家原值，不要换算成统一分数，也不要写总分。

## 无数据不等于通过

Token Sniffer、De.Fi、Quick Intel、DappBay 的文本抓取经常读不到结果，原因是 Cloudflare 验证和 JS 渲染，不是合约没有问题。真浏览器重跑后四家全部有数据。

空值、未知和模拟失败都要标未读到或未核实，不要写成安全通过。

## 测试网合约的特殊性

测试网合约在主网扫描器上本来就没有数据。两个测试网 Token 各有 10 个未识别自定义函数，且均无 owner 函数。在拿到源码前，增发、改税、黑名单、暂停等能力一律记未知，不可记无。

迁移合约和预言机共用一个所有者，三者 factory 指向同一个地址。交易入口不是标准 Pancake V2 Router，预言机没有 Chainlink 风格的 latestRoundData 函数。这些都要在报告中单列，不要只写通过或不通过。
