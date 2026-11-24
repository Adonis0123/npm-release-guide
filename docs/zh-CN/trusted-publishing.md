# 让 GitHub 替你发布（受信发布）

最后核实：2026-11-24

> 状态：正在端到端验证，步骤可能调整。

这一页说明如何把 `your-package` 绑定到 GitHub Actions。配好后，以后发新版本不需要访问令牌（Access Token），也不用弹第二重验证（Second Factor）。

## 什么是受信发布（Trusted Publishing）

受信发布在 npm 上的包和 CI 工作流之间建立绑定。它支持 GitHub Actions（GitHub 托管运行器）、GitLab CI/CD（GitLab.com 共享运行器）和 CircleCI 云端；不支持自托管运行器 [F-01]。只要绑定时允许直接发布，工作流就能直接执行 `npm publish`、`npm stage publish` 和 `npm dist-tag`，不需要访问令牌或第二重验证 [F-10], [F-13]。

## 开始前要满足的条件

- `your-package` 必须已经在 npm 上存在。如果是全新包，先完成 [发布一个全新的包](./first-release.md) [F-05]。
- `npm trust` 需要 npm CLI 11.15.0 及以上，账号要有该包的写权限并开启 2FA [F-06]。你不用自己升级 npm：Agent 给你的那行命令用 `npx --yes npm@11`，会自动取够新的 npm [F-69]。
- GitHub 工作流需要 Node.js 22.14.0 及以上，以及 npm CLI 11.5.1 及以上 [F-02]。

## 一次性配置：Agent 做什么，你做什么

### 代理（Agent）负责做的事

- 从技能模板添加 `release.yml` 工作流，里面带上 `permissions: id-token: write` [F-11] 以及要求的 Node.js 和 npm 版本 [F-02]。
- 创建只允许 `v*` 标签使用的 GitHub `npm` 环境。在我们的实测中，建规则时漏掉 `type=tag` 会被存成分支规则，导致 `v*` 标签无法触发该环境；带上 `type=tag` 和 `v*` 规则后正常生效 [F-63]。
- 核对 `package.json` 里的 `repository.url`，确保与 GitHub 仓库完全一致 [F-11]。
- 发版时用 `npm version` 升版本号，只推送这个版本的标签。
- 准备一行给你在真实终端（Real Terminal）运行的命令。

### 你在真实终端里做的事

每当 npm 需要你的第二重验证，Agent 会给你一行命令，粘贴到真实终端运行，再在自己的浏览器里点确认 [F-43], [F-44]。永远不要把验证码发进 Agent 聊天框。

1. 把 Agent 交给你的那一行粘贴到真实终端运行（不需要自己手敲）。它用 `npx --yes npm@11` 避免本机 npm 过旧 [F-06]，并执行 `npm trust github`，带上 `--file`、`--repo`、可选的 `--env` 和 `--allow-publish` [F-07]：

   ```sh
   npx --yes npm@11 trust github your-package --file release.yml --repo <owner>/<repo> --env npm --allow-publish --yes
   ```

2. 打开终端打印的浏览器确认（Browser Confirmation）链接，用通行密钥（Passkey）确认 [F-08], [F-44]。浏览器页面会提供选项，可在接下来 5 分钟内跳过后续命令的 2FA [F-08]。
3. 收紧包的发布权限，禁止访问令牌发布 `your-package`。在 npmjs.com 包设置里选 "Require two-factor authentication and disallow tokens"，受信发布不受影响 [F-04], [F-26], [F-27]。Agent 也可能把 `npx --yes npm@11 access set mfa=publish your-package` 放进交给你的那一行命令里；该参数大概率对应网页里的这一项，但 npm 文档未正式确认两者的对应关系 [F-28], [F-29]。

### 容易踩坑的细节

- **必须允许直接发布**：2026-09-03 之后新建的受信发布链接默认只允许暂存，不带 `--allow-publish` 时每次发版都只会变成暂存发布（Staged Release），不会直接上线 [F-07], [F-10]。已有链接不能编辑，填错了只能用 `npm trust revoke` 删掉重建 [F-08], [F-10]。
- **2 天内完成首次发布**：新建的受信发布链接必须在 2 天内成功发版一次，否则会过期作废，只能删掉重建 [F-03]。
- **区分大小写**：保存配置时 npm 不会校验对错，所有字段都区分大小写，工作流文件名写错要到发版时才会报 `ENEEDAUTH` [F-12]。
- **一个包能绑几条链接**：两页 npm 文档互相矛盾，一页写 1 条，另一页写最多 10 条，当前实际限制尚未确认 [F-09]。

## 配好之后的日常发版

以后发版时，告诉 Agent 要发哪个版本（比如“release a patch”）。Agent 会升版本号并推送版本标签，GitHub Actions 随即替你完成自动发布（Automated Release）。

## 怎么确认成功了

- 打开 npmjs.com 上的 `your-package` 页面，看新版本是不是已经上线。
- 如果 GitHub 仓库和 npm 包都是公开的，npm 会自动加上构建来源证明（Provenance），不用加 `--provenance` [F-14]。私有仓库不会生成它，哪怕包是公开的也没有 [F-15]。
- 不要用 `npm whoami` 测试受信发布，它不走受信发布 [F-13]。想查看已有绑定，让 Agent 把 `npx --yes npm@11 trust list your-package` 放进一行真实终端命令交给你运行 [F-06], [F-08]。

遇到不熟的词，看 [你会遇到的词](./concepts.md)。

遇到问题：把报错文字粘贴给 Agent，并说“use npm-publish-guide”。

## 事实来源

- [F-01](../facts.md)
- [F-02](../facts.md)
- [F-03](../facts.md)
- [F-04](../facts.md)
- [F-05](../facts.md)
- [F-06](../facts.md)
- [F-07](../facts.md)
- [F-08](../facts.md)
- [F-09](../facts.md)
- [F-10](../facts.md)
- [F-11](../facts.md)
- [F-12](../facts.md)
- [F-13](../facts.md)
- [F-14](../facts.md)
- [F-15](../facts.md)
- [F-26](../facts.md)
- [F-27](../facts.md)
- [F-28](../facts.md)
- [F-29](../facts.md)
- [F-43](../facts.md)
- [F-44](../facts.md)
- [F-63](../facts.md)
- [F-69](../facts.md)

[F-01]: ../facts.md
[F-02]: ../facts.md
[F-03]: ../facts.md
[F-04]: ../facts.md
[F-05]: ../facts.md
[F-06]: ../facts.md
[F-07]: ../facts.md
[F-08]: ../facts.md
[F-09]: ../facts.md
[F-10]: ../facts.md
[F-11]: ../facts.md
[F-12]: ../facts.md
[F-13]: ../facts.md
[F-14]: ../facts.md
[F-15]: ../facts.md
[F-26]: ../facts.md
[F-27]: ../facts.md
[F-28]: ../facts.md
[F-29]: ../facts.md
[F-43]: ../facts.md
[F-44]: ../facts.md
[F-63]: ../facts.md
[F-69]: ../facts.md
