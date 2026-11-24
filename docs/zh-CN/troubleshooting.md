# 按报错文字排错

最后核实：2026-11-24

这一页按你看到的报错文字，说明它是什么意思以及怎么交代代理（Agent）处理。拿不准时，只把报错代码和报错那几行发给 Agent（不要贴整段终端输出，也绝不要发验证码、令牌或确认链接），并说“use npm-publish-guide”。

## 只要记住一条规则

每当 npm 需要你的第二重验证（Second Factor），Agent 会给你一行命令，粘贴到真实终端（Real Terminal）运行，再在自己的浏览器里点确认 [F-43], [F-44]。永远不要把验证码发进 Agent 聊天框。

## 报错 `E401 Unauthorized` / `npm whoami` 失败

- **这是什么意思**：通常说明你的电脑上目前没有有效的登录会话（Login Session） [F-62]。
- **为什么会这样**：在我们的实测中，没有登录会话时运行 `npm whoami` 或 `npm publish` 会报 `E401 Unauthorized`，运行 `npm login` 后恢复正常 [F-62]。
- **怎么跟 Agent 说**：“我遇到 `E401 Unauthorized`，用 npm-publish-guide 给我一行先登录再发布的真实终端命令。”

## Agent 运行时报错 `EOTP` / "OTP required for authentication"

- **这是什么意思**：npm 需要第二重验证，但命令不是在真实终端里跑的 [F-43]。
- **为什么会这样**：输入或输出没有连到交互式终端（`TTY`）时，npm 不会发起浏览器确认（Browser Confirmation），直接报 `EOTP` [F-43]。在我们的实测中，哪怕在真实终端里，把输出通过 `| tee` 管道转走依然会报 `EOTP`；去掉管道后浏览器确认链接才重新出现 [F-64]。
- **怎么跟 Agent 说**：“用 npm-publish-guide 给我准备一行在真实终端运行的命令，不要加管道。”

## 在真实终端里只配了通行密钥（Passkey），却提示 "This operation requires a one-time password." / "Enter OTP:"

- **这是什么意思**：npm 没从服务端拿到浏览器确认链接，退回到让你填一次性动态码（OTP） [F-45]。
- **为什么会这样**：官方没有说明只配通行密钥的发布者（Publisher）何时会遇到 "Enter OTP:" [F-47]。npm CLI 会把 `auth-type` 配置放进 `npm-auth-type` 请求头 [F-46]。按代码推断，常见诱因包括配置了 `auth-type=legacy`、预先设了 `otp` 参数（它会强制切到 `legacy`） [F-36]，或服务端返回里没有浏览器链接 [F-45], [F-47]。
- **先做什么**：在真实终端里按 `Ctrl+C` 中断，不要输入任何内容，然后告诉 Agent。Agent 会检查你的 `auth-type` 配置，并为你重新准备一行真实终端命令 [F-45], [F-46], [F-47]。
- **怎么跟 Agent 说**：“我只配了通行密钥，但真实终端提示 'Enter OTP:'。帮我检查 npm 的 `auth-type` 配置，再用 npm-publish-guide 重新准备一行真实终端命令。”

## 点开浏览器确认链接却报 `E404` 或跳到登录页

- **这是什么意思**：这条浏览器确认链接已经失效，或者打开它的浏览器没有登录 npm [F-42], [F-65]。
- **为什么会这样**：npm 没有公布链接有效期 [F-41]。在一次实测中，链接大约 95 秒后失效并返回 `E404` [F-42]。另外，在我们的实测中，在未登录 npm 的浏览器里打开链接会跳去登录页而不是确认页，等登录完链接已经过期；改在发布者自己已登录的浏览器打开则正常通过 [F-65]。
- **怎么跟 Agent 说**：“浏览器链接超时或跳登录页了，把那一行再给我一次，我跑完马上在已登录的浏览器里打开。”

## GitHub Actions 发布时报 `ENEEDAUTH` / "Unable to authenticate"

- **这是什么意思**：npm 拒绝了工作流提交的受信发布（Trusted Publishing）身份 [F-12]。
- **为什么会这样**：保存受信发布配置时 npm 不做校验，且所有字段区分大小写 [F-12]。可以按下面几项排查（npm 仅明确确认工作流文件名写错会报此错 [F-12]）：
  - 工作流文件名、仓库名或环境名与受信发布配置不一致 [F-07], [F-12]。
  - `package.json` 里的 `repository.url` 与 GitHub 仓库没有完全匹配，或工作流缺少 `permissions: id-token: write` [F-11]。
  - 工作流里的 Node.js 低于 22.14.0，或 npm CLI 低于 11.5.1 [F-02]。
  - 新建的受信发布链接 2 天内没有成功发过版，已经过期 [F-03]。
- **怎么跟 Agent 说**：“GitHub Actions 报 ENEEDAUTH。帮我核对 `release.yml`、`package.json` 的 `repository.url`、Node 和 npm 版本，再给我一行真实终端命令运行 `npx --yes npm@11 trust list your-package`。” [F-06], [F-08]

## GitHub Actions 发上去成了暂存发布（Staged Release），没有直接上线

- **这是什么意思**：这条受信发布链接只允许暂存，没有开启直接 `npm publish` [F-10]。
- **为什么会这样**：2026-09-03 之后创建的受信发布链接默认只允许暂存，必须带上 `--allow-publish` 才能直接发布 [F-07], [F-10]。已有链接无法编辑 [F-10]。
- **怎么跟 Agent 说**：“我的受信发布链接只会暂存，给我一行命令删掉旧链接并带上 `--allow-publish` 重建。”（当前挂着的版本可以先用 `npm stage approve <stage-id>` 或在 npmjs.com 批准 [F-50]。）

## 用访问令牌（Access Token）发布时报 `E_STAGE_REQUIRED`

- **这是什么意思**：这个访问令牌选的是 "Read and write (stage only)" [F-24]。
- **为什么会这样**：自 2026-09-18 起，只允许暂存的访问令牌能跑 `npm stage publish`，但跑 `npm publish` 会报 `E_STAGE_REQUIRED`，就算开了 Bypass 2FA 也不行 [F-24]。
- **怎么跟 Agent 说**：“帮我把 `your-package` 改成受信发布，不再用访问令牌。”

## GitHub 环境拒绝了 `v*` 标签

- **这是什么意思**：GitHub 的 `npm` 环境拦住了这个标签，没让发布工作流运行。
- **为什么会这样**：在我们的实测中，创建 GitHub 环境规则时漏掉 `type=tag` 会被存成分支规则，推送 `v*` 标签时就无法使用该环境；改成 `type=tag` 和 `v*` 规则后正常通过 [F-63]。
- **怎么跟 Agent 说**：“GitHub 的 `npm` 环境拒绝了 `v*` 标签，帮我检查部署规则，把分支规则改成 `type=tag` 的 `v*` 规则。”

## npm 包页面显示的 README 语言不对

- **这是什么意思**：npm 在包页面上展示了你不想默认展示的那份 README。
- **为什么会这样**：在我们的实测中，包目录同时包含 `README.md` 和 `README.zh-CN.md` 时，npm 包页面显示了中文版；在发布步骤里明确指定把 `README.md` 作为 readme 后恢复正常 [F-66]。
- **怎么跟 Agent 说**：“npm 包页面显示的 README 语言不对，帮我改一下发布步骤，明确指定用 `README.md`。”

## 账号被冻结 72 小时

- **这是什么意思**：npm 对你的账号加了 72 小时临时安全冻结 [F-34]。
- **为什么会这样**：用恢复码登录会触发 72 小时冻结，期间不能发包、不能建令牌、也不能改设置，且无法提前解除 [F-34]。
- **怎么跟 Agent 说**：“我的 npm 账号进了 72 小时恢复码冻结期，记一下解冻时间，到期再继续。”

遇到问题：只把报错代码和报错那几行发给 Agent（不要贴整段终端输出，也绝不要发验证码、令牌或确认链接），并说“use npm-publish-guide”。

## 事实来源

- [F-02](../facts.md)
- [F-03](../facts.md)
- [F-06](../facts.md)
- [F-07](../facts.md)
- [F-08](../facts.md)
- [F-10](../facts.md)
- [F-11](../facts.md)
- [F-12](../facts.md)
- [F-24](../facts.md)
- [F-34](../facts.md)
- [F-36](../facts.md)
- [F-41](../facts.md)
- [F-42](../facts.md)
- [F-43](../facts.md)
- [F-44](../facts.md)
- [F-45](../facts.md)
- [F-46](../facts.md)
- [F-47](../facts.md)
- [F-50](../facts.md)
- [F-62](../facts.md)
- [F-63](../facts.md)
- [F-64](../facts.md)
- [F-65](../facts.md)
- [F-66](../facts.md)

[F-02]: ../facts.md
[F-03]: ../facts.md
[F-06]: ../facts.md
[F-07]: ../facts.md
[F-08]: ../facts.md
[F-10]: ../facts.md
[F-11]: ../facts.md
[F-12]: ../facts.md
[F-24]: ../facts.md
[F-34]: ../facts.md
[F-36]: ../facts.md
[F-41]: ../facts.md
[F-42]: ../facts.md
[F-43]: ../facts.md
[F-44]: ../facts.md
[F-45]: ../facts.md
[F-46]: ../facts.md
[F-47]: ../facts.md
[F-50]: ../facts.md
[F-62]: ../facts.md
[F-63]: ../facts.md
[F-64]: ../facts.md
[F-65]: ../facts.md
[F-66]: ../facts.md
