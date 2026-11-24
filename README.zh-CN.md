# npm-release-guide

[English](README.md)

最后核实：2026-11-24

选对版本号并发布 npm 包，不再卡在两步验证上。不会写代码也能用：遇到问题交给 Agent 处理。

## 从这里开始

把两个技能装进你的项目（推荐；需要 Node.js 22.20 及以上 [F-61]）：

```sh
npx skills add Adonis0123/npm-release-guide
```

这条命令会从 `skills/*/SKILL.md` 把 `npm-publish-guide` 和 `npm-version-guide` 装进当前项目 [F-60], [F-111]。把装好的目录和 `skills-lock.json` 一起提交到仓库，同事拉下来就能直接用 [F-111], [F-112]。

- 只装其中一个：`npx skills add Adonis0123/npm-release-guide --skill npm-version-guide`（或 `--skill npm-publish-guide`；实测可用 [F-113]）
- 以后更新：`npx skills update` [F-112]
- 锁定版本：`npx skills add Adonis0123/npm-release-guide@v0.1.0`（据 CLI 文档，暂未实测 [F-113]）

Claude Code 用户也可以改用插件方式安装 [F-114]（第三方插件市场的自动更新默认关闭 [F-115]）：

```text
/plugin marketplace add Adonis0123/npm-release-guide
/plugin install npm-release-guide@npm-release-guide
```

然后对你的 AI 编程助手（Agent，比如 Claude Code、Codex、Cursor）说：

- 要发包：`用 npm-publish-guide 帮我把这个项目发布到 npm。`
- 要选版本号：`下个版本该升哪个号？用 npm-version-guide 看看。`

卡住了：把报错原文粘贴给 Agent，再说一句“用 npm-publish-guide 看看”（或“用 npm-version-guide 看看”）。

## 项目配置

第一次在项目里用技能时，Agent 会先看你的仓库，展示它推算出的配置，只问一次“是 / 否”要不要存进 `.agents/release.json`。把这个文件提交到仓库，以后它就会直接沿用。配置说明见 [项目配置](docs/zh-CN/config.md)。

## 只要记住一条规则

每当 npm 需要你的第二重验证（Second Factor），Agent 会给你一行命令，粘贴到真实终端（Real Terminal）运行，再在自己的浏览器里点确认 [F-43], [F-44]。永远不要把验证码发进 Agent 聊天框。

## 整个流程

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/images/publish-flow.zh-CN.dark.svg">
  <img alt="流程：告诉 AI，AI 检查包；新包先在你自己的终端首次发布，再绑定一次 GitHub 和 npm；之后每次发版由 AI 推送 tag，GitHub 自动发布" src="docs/images/publish-flow.zh-CN.light.svg">
</picture>

## 想自己弄明白？

| 你的情况 | 看这一页 |
| --- | --- |
| 不确定下个版本该升哪个号 | [选版本号](docs/zh-CN/versioning.md) |
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
- [F-111](docs/facts.md)
- [F-112](docs/facts.md)
- [F-113](docs/facts.md)
- [F-114](docs/facts.md)
- [F-115](docs/facts.md)

[F-43]: docs/facts.md
[F-44]: docs/facts.md
[F-60]: docs/facts.md
[F-61]: docs/facts.md
[F-111]: docs/facts.md
[F-112]: docs/facts.md
[F-113]: docs/facts.md
[F-114]: docs/facts.md
[F-115]: docs/facts.md
