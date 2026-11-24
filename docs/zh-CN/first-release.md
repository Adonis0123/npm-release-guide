# 发布一个全新的包

最后核实：2026-11-24

这一页带你把 `your-package` 的第一个版本发到 npm。每个新包只用走这一次。

## 为什么第一版必须从你电脑发

受信发布（Trusted Publishing）只能绑到已经在 npm 上存在的包 [F-05]。所以首次发布（First Release）必须作为本地发布（Local Release），从你的电脑发出。

## 只要记住一条规则

每当 npm 需要你的第二重验证（Second Factor），代理（Agent）会给你一行命令，粘贴到真实终端（Real Terminal）运行，再在自己的浏览器里点确认 [F-43], [F-44]。永远不要把验证码发进 Agent 聊天框。

## 每一步你会看到什么

1. **告诉 Agent**：对它说“Publish this project to npm using npm-publish-guide.”。
2. **Agent 帮你检查包**：它先看包名有没有被占，再核对要打进包里的文件。
3. **Agent 准备一行命令**：它把要跑的命令整理成一行，复制到剪贴板或打印给你。
4. **粘贴到真实终端**：打开你自己的终端窗口，粘贴这一行，按回车。
5. **需要时登录**：如果电脑上没有有效的登录会话（Login Session），`npm login` 会打印 "Authenticate your account at:" 和一条链接 [F-36], [F-37], [F-38]。按回车打开浏览器登录。
6. **在浏览器确认第二重验证**：`npm publish` 需要第二重验证时，通常会打印浏览器确认（Browser Confirmation）链接 [F-44]；有时也会提示 "Enter OTP:" [F-45], [F-47]（如果只配了通行密钥，见[按报错文字排错](./troubleshooting.md)）。拿到链接后马上在自己的浏览器打开，用通行密钥（Passkey）确认。npm 没有公布链接有效期；一次实测中大约 95 秒后失效 [F-41], [F-42]。

遇到看不懂的词，先看 [你会遇到的词](./concepts.md)。

## 发布后可能看到什么

暂存一个全新包名时，npm 会先发一个公开占位版本 `0.0.0-stage`，直到批准为止 [F-51]。npm 文档没有写正常发布是否会出现它；但在一次新包首次发布的实测中，`0.0.0-stage` 曾短暂出现约 3 分钟 [F-52]。

## 下一步

现在 `your-package` 已经在 npm 上了。接着去配 [受信发布](./trusted-publishing.md)，以后让 GitHub 替你发新版本，不用再弹验证窗口。

遇到问题：把报错文字粘贴给 Agent，并说“use npm-publish-guide”。

## 事实来源

- [F-05](../facts.md)
- [F-36](../facts.md)
- [F-37](../facts.md)
- [F-38](../facts.md)
- [F-41](../facts.md)
- [F-42](../facts.md)
- [F-43](../facts.md)
- [F-44](../facts.md)
- [F-45](../facts.md)
- [F-47](../facts.md)
- [F-51](../facts.md)
- [F-52](../facts.md)

[F-05]: ../facts.md
[F-36]: ../facts.md
[F-37]: ../facts.md
[F-38]: ../facts.md
[F-41]: ../facts.md
[F-42]: ../facts.md
[F-43]: ../facts.md
[F-44]: ../facts.md
[F-45]: ../facts.md
[F-47]: ../facts.md
[F-51]: ../facts.md
[F-52]: ../facts.md
