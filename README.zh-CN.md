# npm-publish-guide

[English](README.md)

最后核实：2026-11-24

发布 npm 包，不再卡在两步验证上。不会写代码也能用：遇到问题交给 AI 去处理。

## 从这里开始

安装技能（需要 Node.js 22.20 及以上 [F-61]）：

```sh
npx skills add Adonis0123/npm-publish-guide
```

这条命令会从 `skills/npm-publish-guide/SKILL.md` 安装 `npm-publish-guide` 技能 [F-60]。

然后对你的 AI 编程助手（Agent，比如 Claude Code、Codex、Cursor）说：

> 用 npm-publish-guide 帮我把这个项目发布到 npm。

卡住了：把报错原文粘贴给 Agent，再说一句“用 npm-publish-guide 看看”。

## 只要记住一条规则

每当 npm 需要你的第二重验证（Second Factor），Agent 会给你一行命令，粘贴到真实终端（Real Terminal）运行，再在自己的浏览器里点确认 [F-43], [F-44]。永远不要把验证码发进 Agent 聊天框。

## 想自己弄明白？

| 你的情况 | 看这一页 |
| --- | --- |
| 发布一个全新的包 | [首次发布](docs/zh-CN/first-release.md) |
| 从自己电脑发新版本 | [本机发版](docs/zh-CN/local-release.md) |
| 想让 GitHub 替你发布，不再弹验证 | [自动发布](docs/zh-CN/trusted-publishing.md) |
| 发布卡在某个报错上 | [按报错排障](docs/zh-CN/troubleshooting.md) |
| 看不懂 npm 的两步验证设置页 | [设置页逐项解释](docs/zh-CN/settings-page.md) |
| 有不认识的词 | [名词解释](docs/zh-CN/concepts.md) |

## 适用范围

本指南只讲发布到 npm（registry.npmjs.org）。pnpm、yarn、bun 发包也走这里，区别写在[名词解释](docs/zh-CN/concepts.md)里。JSR、PyPI 不在范围内。

## 开源协议

MIT

## 事实来源

- [F-43](docs/facts.md)
- [F-44](docs/facts.md)
- [F-60](docs/facts.md)
- [F-61](docs/facts.md)

[F-43]: docs/facts.md
[F-44]: docs/facts.md
[F-60]: docs/facts.md
[F-61]: docs/facts.md
