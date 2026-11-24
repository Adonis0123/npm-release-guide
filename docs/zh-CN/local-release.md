# 从自己电脑发新版本

最后核实：2026-11-24

这一页说明如何在自己电脑上对 `your-package` 做本地发布（Local Release）。也说明为什么代理（Agent）不能在后台替你跑最后一步。

## 什么时候还需要本地发布

遇到这些情况时，仍需从自己电脑操作：

- 还没配好受信发布（Trusted Publishing）。
- 要做受信发布不支持的操作，比如批准暂存发布（Staged Release），即 `npm stage approve` [F-13], [F-50]。
- 账号开着默认的“授权与写入”2FA，需要弃用版本、撤回版本或修改包权限 [F-30]。

## 只要记住一条规则

每当 npm 需要你的第二重验证（Second Factor），Agent 会给你一行命令，粘贴到真实终端（Real Terminal）运行，再在自己的浏览器里点确认 [F-43], [F-44]。永远不要把验证码发进 Agent 聊天框。

## 操作流程

1. 让 Agent 用 `npm-publish-guide` 准备一次本地发布。
2. Agent 会整理好一行命令交给你。
3. 打开你的真实终端，粘贴这一行并按回车。
4. 如果登录会话（Login Session）过期了，`npm login` 会打印链接，让你在浏览器里重新登录 [F-36], [F-37]。更新日志写明登录会话有效 2 小时 [F-38]；有 npm 工作人员在论坛说已延长到 12 小时，但官方文档尚未确认 [F-39]。
5. `npm publish` 要第二重验证时，打开终端里的浏览器确认（Browser Confirmation）链接，用通行密钥（Passkey）确认 [F-44]。尽量马上点开：npm 没公布超时时间，一次实测中约 95 秒后失效 [F-41], [F-42]。

## 为什么 Agent 在后台跑会报 `EOTP`

npm 需要第二重验证时，会检查输入和输出是否都连着交互式终端（`TTY`） [F-43]。Agent 跑命令的环境不是真实终端，npm 不会打印链接，也不会等你输入，直接报 `EOTP`（"OTP required for authentication"） [F-43]。

## 为什么加管道（比如 `| tee`）也会失败

就算在你的真实终端里，只要把 `npm publish` 的输出用管道（例如 `| tee`）转走，标准输出就不再是终端（`TTY`）。npm 会命中同一条检查，不打印浏览器确认链接，直接报 `EOTP` [F-43]。在我们的实测中，把输出通过 `| tee` 转走的交接脚本在真实终端里依然报 `EOTP`，去掉管道后浏览器确认链接才重新出现 [F-64]。直接运行 Agent 给的那一行，不要额外加管道。

以后不想再手动从电脑发版，去配 [受信发布](./trusted-publishing.md)。遇到不熟的词，看 [你会遇到的词](./concepts.md)。

遇到问题：把报错文字粘贴给 Agent，并说“use npm-publish-guide”。

## 事实来源

- [F-13](../facts.md)
- [F-30](../facts.md)
- [F-36](../facts.md)
- [F-37](../facts.md)
- [F-38](../facts.md)
- [F-39](../facts.md)
- [F-41](../facts.md)
- [F-42](../facts.md)
- [F-43](../facts.md)
- [F-44](../facts.md)
- [F-50](../facts.md)
- [F-64](../facts.md)

[F-13]: ../facts.md
[F-30]: ../facts.md
[F-36]: ../facts.md
[F-37]: ../facts.md
[F-38]: ../facts.md
[F-39]: ../facts.md
[F-41]: ../facts.md
[F-42]: ../facts.md
[F-43]: ../facts.md
[F-44]: ../facts.md
[F-50]: ../facts.md
[F-64]: ../facts.md
