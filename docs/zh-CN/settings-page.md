# npm 两步验证设置页逐项说明

最后核实：2026-11-24

这一页用文字逐项讲清 npm 的两步验证设置页。也说明配好受信发布（Trusted Publishing）后，包级别该开哪一项。

## 推荐配置

- 账号第二重验证（Second Factor）：只用通行密钥（Passkey） [F-32]。
- 账号模式：保持 "authorization and writes" 开启（网页上是否写作 "Require two-factor authentication for write actions" 尚未经 npm 文档确认） [F-30], [F-31]。
- 包级发布权限（配好受信发布后）：选 "Require two-factor authentication and disallow tokens" [F-04], [F-26]。
- 恢复码（Recovery Codes）：下载并存进密码管理器 [F-34]。

## 账号级设置

### Security Keys（你的通行密钥）

npm 网页把通行密钥叫做 "security key" [F-32]。它只能在网页里添加，配好后网页和命令行都能用 [F-32]。

我们实测看到：已经有一把 security key 时，页面上仍有 "Add Security Key" 按钮，所以可以再加一个通行密钥作备用 [F-67]。

只用通行密钥时，不需要装动态码应用。每当命令行需要第二重验证，代理（Agent）会给你一行命令，粘贴到真实终端（Real Terminal）运行，再在自己的浏览器里点确认 [F-43], [F-44]。不过某些情况下 npm 仍可能提示 "Enter OTP:" [F-47]；遇到时请看[按报错文字排错](./troubleshooting.md)。永远不要把验证码发进 Agent 聊天框。

### Recovery Codes（恢复码）

开启 2FA 时 npm 会给出一组恢复码，之后也能在 2FA 设置页查看或重新生成 [F-34]。把它们下载下来，放进密码管理器。

每个恢复码只能用一次 [F-34]。平时不要用它登录：用恢复码登录会触发 72 小时安全冻结 [F-34]。冻结期间不能发包、不能创建访问令牌（Access Token）、也不能改设置，而且无法提前解除 [F-34]。

### "Require two-factor authentication for write actions"（该文案尚未经 npm 文档确认 [F-31]）

这一项保持开启。已核查的 npm 文档页面里没有出现这句原话，它是否完全等同于 `auth-and-writes` 尚未经官方文档确认 [F-31]。

npm 文档列了两种账号 2FA 模式 [F-30]：

- **"authorization and writes"（`auth-and-writes`，默认）**：登录、改资料、改令牌要第二重验证；发包、撤包、弃用和改包权限也要第二重验证 [F-30]。
- **"authorization only"（`auth-only`）**：只有登录、改资料和改令牌时才要第二重验证 [F-30]。

## 包级设置：Publishing access

每个包都有单独的 "Publishing access" 设置，包含两个选项 [F-26]：

- **"Require two-factor authentication or a granular access token with bypass 2fa enabled"**：所有新包的默认选项 [F-26]。
- **"Require two-factor authentication and disallow tokens"**（推荐）：禁止任何访问令牌发布 `your-package`，哪怕令牌开了 Bypass 2FA 也不行 [F-26], [F-27]。

配好 [受信发布](./trusted-publishing.md) 后，把这一项切到 "Require two-factor authentication and disallow tokens"。它只拦访问令牌，受信发布照常工作 [F-04]。

你不需要自己手敲命令行：如果 Agent 在交给你的真实终端一行命令里放了 `npx --yes npm@11 access set mfa=publish your-package`（用 `npx` 是为了避免本机 npm 版本过旧 [F-06]），请注意：npm 文档没有明确写它和网页选项的对应关系，但 CLI 源码会发送 `publish_requires_tfa=true` 和 `automation_token_overrides_tfa=false` [F-28], [F-29]。

遇到问题：把报错文字粘贴给 Agent，并说“use npm-publish-guide”。

## 事实来源

- [F-04](../facts.md)
- [F-06](../facts.md)
- [F-26](../facts.md)
- [F-27](../facts.md)
- [F-28](../facts.md)
- [F-29](../facts.md)
- [F-30](../facts.md)
- [F-31](../facts.md)
- [F-32](../facts.md)
- [F-34](../facts.md)
- [F-43](../facts.md)
- [F-44](../facts.md)
- [F-47](../facts.md)
- [F-67](../facts.md)

[F-04]: ../facts.md
[F-06]: ../facts.md
[F-26]: ../facts.md
[F-27]: ../facts.md
[F-28]: ../facts.md
[F-29]: ../facts.md
[F-30]: ../facts.md
[F-31]: ../facts.md
[F-32]: ../facts.md
[F-34]: ../facts.md
[F-43]: ../facts.md
[F-44]: ../facts.md
[F-47]: ../facts.md
[F-67]: ../facts.md
