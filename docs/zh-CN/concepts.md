# 你会遇到的词

最后核实：2026-11-24

这一页解释发布 npm 包时常见的词。不用背下来，遇到看不懂的词再来查就行。

## 第二重验证（Second Factor）

登录账号后，npm 还会再要一次身份证明，这就是第二重验证。在 npm 上，它要么是通行密钥（Passkey），要么是一次性动态码（OTP）。

- **通行密钥（Passkey）**：在浏览器里用指纹、手机或硬件钥匙确认。npm 网页上把它叫做 "security key"，只能在网页里配置 [F-32]。
- **一次性动态码（OTP）**：身份验证器应用生成的一次性动态码 [F-33]。如果你只配了通行密钥，手里就没有动态码可填。

你会在这里遇到：登录账号、改账号设置，或者做本地发布（Local Release）时 [F-30]。

## 浏览器确认（Browser Confirmation）

浏览器确认是指 npm 在终端里打印一条链接 [F-37], [F-44]。你在自己已登录的浏览器里打开它，再用通行密钥点确认。

`npm login` 默认走浏览器确认 [F-36]。npm 没有公布这条链接多久失效 [F-41]。在一次实测中，它大约 95 秒后失效，晚打开会报 `E404` [F-42]。

你会在这里遇到：在真实终端（Real Terminal）里登录或确认本地发布时。

## 登录会话（Login Session）

登录会话是 `npm login` 存在电脑上的短期凭证 [F-38]。后面的 npm 命令靠它确认你的身份。

npm 更新日志写明登录会话有效 2 小时 [F-38]。一名 npm 工作人员在论坛回复称已延长到 12 小时，但没有官方文档或更新日志确认这一点 [F-39]。（有一页 npm CLI 文档仍写着 `npm login` 会生成旧令牌，这与更新日志冲突，看上去已过时 [F-40]。）

你会在这里遇到：隔了一段时间再发布，电脑提示重新登录时。

## 访问令牌（Access Token）

访问令牌是一串长期有效的密钥，让机器替你操作。本指南只在万不得已时才提它。

现在只剩细粒度访问令牌，所有旧版 classic token 已在 2025-12-09 永久作废 [F-16], [F-17]。带写权限的访问令牌最长 90 天有效 [F-18]。2025 年曾预告默认有效期为 7 天，但目前没有官方文档确认这一默认值是否仍在生效 [F-19]。

写权限访问令牌可以勾选 "Bypass two-factor authentication"（默认关闭） [F-20]。自 2026 年 8 月起，带 Bypass 2FA 的令牌不能再改 2FA、管令牌、增删成员、管组织或团队，也不能改受信发布（Trusted Publishing）配置 [F-21], [F-22]。npm 文档写明，2027 年 1 月将移除用细粒度访问令牌直接发包的能力 [F-22], [F-23]。自 2026-09-18 起，访问令牌新增 "Read and write (stage only)" 选项：能跑 `npm stage publish`，但跑 `npm publish` 会报 `E_STAGE_REQUIRED` [F-24]。

你会在这里遇到：旧教程让你把一串密钥贴进 CI 后台时。

## 受信发布（Trusted Publishing）

受信发布是你的包与 CI 工作流之间的绑定。它支持 GitHub Actions（GitHub 托管运行器）、GitLab CI/CD（GitLab.com 共享运行器）和 CircleCI（CircleCI 云端）；不支持自托管运行器 [F-01]。

工作流里需要 Node.js 22.14.0 及以上，以及 npm CLI 11.5.1 及以上 [F-02]。绑定前，包必须已经在 npm 上存在 [F-05]。开启直接发布后，工作流可以直接跑 `npm publish`、`npm stage publish` 和 `npm dist-tag`，不需要访问令牌，也不用第二重验证 [F-10], [F-13]。

你会在这里遇到：想让 GitHub 以后替你自动发新版本时。

## 自动发布（Automated Release）

自动发布是指 CI 工作流通过受信发布发出的版本。通常推送一个版本标签就会触发它。

你会在这里遇到：配好受信发布后的每一次日常发版。

## 暂存发布（Staged Release）

暂存发布是用 `npm stage publish` 先把版本传上去，等你做完第二重验证才正式上线 [F-50]。它在 2026-05-22 正式上线 [F-48]，需要 npm CLI 11.15.0 及以上、Node.js 22.14.0 及以上、包的发布权限，以及账号开启 2FA [F-49]。

你可以先用 `npm stage list`、`view`、`download` 检查内容，再用 `npm stage approve <stage-id>` 或在 npmjs.com 网页点批准 [F-50]。暂存版本的 dist-tag 在上传时就定死了 [F-53]。暂存一个全新包名时，npm 会先发一个公开占位版本 `0.0.0-stage`，直到你批准 [F-51]。

你会在这里遇到：新建受信发布链接时忘了开直接发布 [F-10]，或者你想先检查再上线时。

## 构建来源证明（Provenance）

构建来源证明是附在包版本上的公开记录，写明这个版本来自哪个仓库、哪条工作流。

从公开仓库通过受信发布发公开包时，npm 会自动加上构建来源证明，不用传 `--provenance` [F-14]。私有仓库和 CircleCI 不会生成它，就算包本身是公开的也没有 [F-14], [F-15]。

你会在这里遇到：用公开 GitHub 仓库做完自动发布后，版本元数据里会带上构建来源证明（在我们的实测中，`npm view` 显示了 `dist.attestations.provenance` [F-68]）。

## 真实终端（Real Terminal）和代理（Agent）

**真实终端（Real Terminal）**是你自己打开、自己敲命令的终端窗口。**代理（Agent）**是替你干活的 AI 编程工具。

npm 需要第二重验证时，会检查输入输出是不是连在真实终端上 [F-43]。Agent 在后台跑命令时并没有真实终端，npm 不会弹窗，直接报 `EOTP` [F-43]。换到你的真实终端，npm 通常会打印浏览器确认链接，让你用通行密钥批准 [F-44]；有时也会退回提示 "Enter OTP:" [F-45], [F-47]。

只要记住一条规则：每当 npm 需要第二重验证，Agent 会给你一行命令，粘贴到真实终端运行，再在自己的浏览器里点确认。永远不要把验证码发进 Agent 聊天框。

## 其他包管理器：pnpm、yarn、bun

这三个工具都可以发布到 npm 仓库，但在登录、第二重验证提示和构建来源证明上各有差别 [F-54], [F-55], [F-56], [F-58], [F-59]。

| 工具 | 登录与凭证 | 发布时的第二重验证 | 构建来源证明 |
| --- | --- | --- | --- |
| pnpm | 读取 npm 的 `~/.npmrc`，npm 的登录会话能直接用 [F-54] | 支持 `--otp`（或 `PNPM_CONFIG_OTP`）；需要浏览器确认时会打印链接和二维码 [F-55] | 受信发布下自动生成 [F-55] |
| yarn (Berry) | `yarn npm login` 要求用户名、密码和 OTP，凭证存进 Yarn 自己的配置；`--web-login` 只用于登录 [F-56]。Yarn v1/v2 用的旧登录接口计划移除 [F-57] | `yarn npm publish` 文档列出 `--otp`、`--provenance`、`--staged`；是否支持浏览器确认在文档中未确认 [F-56] | 支持 `--provenance` [F-56] |
| bun | 读取 `.npmrc` 和 `bunfig.toml`，支持 `NPM_CONFIG_TOKEN` [F-58] | 默认走浏览器确认（`--auth-type web`），也支持 `--auth-type legacy` 和 `--otp` [F-58] | 暂不支持生成，强行开启会报错 [F-59] |

遇到问题：把报错文字粘贴给 Agent，并说“use npm-publish-guide”。

## 事实来源

- [F-01](../facts.md)
- [F-02](../facts.md)
- [F-05](../facts.md)
- [F-10](../facts.md)
- [F-13](../facts.md)
- [F-14](../facts.md)
- [F-15](../facts.md)
- [F-16](../facts.md)
- [F-17](../facts.md)
- [F-18](../facts.md)
- [F-19](../facts.md)
- [F-20](../facts.md)
- [F-21](../facts.md)
- [F-22](../facts.md)
- [F-23](../facts.md)
- [F-24](../facts.md)
- [F-30](../facts.md)
- [F-32](../facts.md)
- [F-33](../facts.md)
- [F-36](../facts.md)
- [F-37](../facts.md)
- [F-38](../facts.md)
- [F-39](../facts.md)
- [F-40](../facts.md)
- [F-41](../facts.md)
- [F-42](../facts.md)
- [F-43](../facts.md)
- [F-44](../facts.md)
- [F-45](../facts.md)
- [F-47](../facts.md)
- [F-48](../facts.md)
- [F-49](../facts.md)
- [F-50](../facts.md)
- [F-51](../facts.md)
- [F-53](../facts.md)
- [F-54](../facts.md)
- [F-55](../facts.md)
- [F-56](../facts.md)
- [F-57](../facts.md)
- [F-58](../facts.md)
- [F-59](../facts.md)
- [F-68](../facts.md)

[F-01]: ../facts.md
[F-02]: ../facts.md
[F-05]: ../facts.md
[F-10]: ../facts.md
[F-13]: ../facts.md
[F-14]: ../facts.md
[F-15]: ../facts.md
[F-16]: ../facts.md
[F-17]: ../facts.md
[F-18]: ../facts.md
[F-19]: ../facts.md
[F-20]: ../facts.md
[F-21]: ../facts.md
[F-22]: ../facts.md
[F-23]: ../facts.md
[F-24]: ../facts.md
[F-30]: ../facts.md
[F-32]: ../facts.md
[F-33]: ../facts.md
[F-36]: ../facts.md
[F-37]: ../facts.md
[F-38]: ../facts.md
[F-39]: ../facts.md
[F-40]: ../facts.md
[F-41]: ../facts.md
[F-42]: ../facts.md
[F-43]: ../facts.md
[F-44]: ../facts.md
[F-45]: ../facts.md
[F-47]: ../facts.md
[F-48]: ../facts.md
[F-49]: ../facts.md
[F-50]: ../facts.md
[F-51]: ../facts.md
[F-53]: ../facts.md
[F-54]: ../facts.md
[F-55]: ../facts.md
[F-56]: ../facts.md
[F-57]: ../facts.md
[F-58]: ../facts.md
[F-59]: ../facts.md
[F-68]: ../facts.md
