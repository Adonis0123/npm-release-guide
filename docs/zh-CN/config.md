# 项目配置：`.agents/release.json`

最后核实：2026-11-24

本指南的两个技能共用项目里的一个小配置文件，用来记住这个项目自己的选择：由哪个工作流发布、运行在哪个 GitHub 环境、`0.x` 版本怎么升号，以及预发布版本（Pre-release）用什么名字。这个文件是可选的。没有它，技能也会推算出同样的值，并且在任何会发到外面的操作前先问你。

## 放在哪里

放在 Git 仓库根目录的 `.agents/release.json`（也可以放在某个子包的文件夹里；从子包目录一路向上找到 Git 根目录，最近的一份生效）。把它提交到仓库，这样每个 Agent 和每位同事都会用同一套配置。

JSON Schema（用于让编辑器自动检查写法）：`https://raw.githubusercontent.com/Adonis0123/npm-release-guide/main/schemas/release.v1.schema.json`。把这个地址填进 `$schema`，编辑器就会在你输入时实时检查。文件名里带有格式版本号（`v1`），所以这个地址的含义以后不会变化。

## 字段说明

```json
{
  "$schema": "https://raw.githubusercontent.com/Adonis0123/npm-release-guide/main/schemas/release.v1.schema.json",
  "version": 1,
  "publish": { "workflow": "release.yml", "environment": "npm", "prereleaseTag": "next" },
  "versioning": { "zeroMajor": "minor", "preid": "beta" }
}
```

| 字段 | 默认值 | 含义 |
| --- | --- | --- |
| `version` | 必填，`1` | 配置格式的版本号。 |
| `publish.workflow` | `release.yml` | 负责发布的 GitHub 工作流文件名。必须和 `npm trust github` 里的 `--file` 一致 [F-07]，npm 会逐字精确比对（区分大小写）[F-12]。 |
| `publish.environment` | `npm` | 发布任务所在的 GitHub 环境名。必须和 `npm trust github` 里的 `--env` 一致 [F-07]。 |
| `publish.prereleaseTag` | `next` | 预发布版本（Pre-release）使用的发布标签（Release Tag）。`release.yml` 模板里写死了 `--tag next`，要改就两边一起改。长得像版本范围的名字（例如 `v1.4`）会被 npm 拒绝 [F-89]。 |
| `versioning.zeroMajor` | `minor` | `minor`：在 `0.x` 阶段，破坏性变更（Breaking Change）升次版本号，新功能和修复升修订号，与 `^0.y.z` 依赖范围的规则一致 [F-94], [F-102]。`major`：按 `1.x` 的规则处理，出现破坏性变更就升到 `1.0.0`。 |
| `versioning.preid` | `beta` | 预发布版本的名称，就像 `npm version preminor --preid beta` 里的 `beta` [F-85]。 |
| `versioning.tagPrefix` | `v` | 发布用的 git 标签前缀。`<包名>@<版本号>` 形式的标签也会被识别。 |
| `versioning.publicInterface` | `exports`、`main`、`module`、`types`、`bin` 指向的文件 | 相对于包目录的路径或通配模式。只要 `fix`、`perf` 或 `refactor` 提交改动了这些文件，就会交给 Agent 复查。如果包入口指向构建产物（例如 `dist/`），而你希望检查源代码，就设置这一项，例如 `["src/index.ts", "src/cli/**"]`。 |
| `packages[]` | 无 | 多包仓库（monorepo）按包覆盖配置，见下文。 |

除了 `$schema` 之外，schema 会拒绝任何未定义的字段。

## 多包仓库（monorepo）：每个包一条配置

顶层的 `publish` 和 `versioning` 是默认值。`packages[]` 里的每一项格式为 `{ "dir", "publish"?, "versioning"? }`，用来覆盖对应目录的默认值。受信发布（Trusted Publishing）是按单个包绑定的 [F-07]，所以同一个仓库里的两个包可以使用不同的工作流或环境。

```json
{
  "version": 1,
  "publish": { "workflow": "release.yml", "environment": "npm" },
  "packages": [
    { "dir": "packages/core" },
    { "dir": "packages/cli", "publish": { "workflow": "release-cli.yml" }, "versioning": { "zeroMajor": "major" } }
  ]
}
```

`dir` 是相对于 `.agents/` 所在目录的路径。运行版本号脚本时传入 `--package <dir>`，它就只读取改动过该目录的提交。`v*` 标签一次只发布一个包；如果要一次联动发布多个包，请改用 Changesets（注意 Changesets 不支持暂存发布 [F-107]）。

## 没有配置文件时，Agent 会怎么推算

| 配置项 | 推算来源 |
| --- | --- |
| `publish.workflow` | `.github/workflows/` 里第一个运行 `npm publish`（或 pnpm、yarn、bun 对应发布命令）的工作流文件 |
| `publish.environment` | 那个工作流里的 `environment:` 这一行 |
| 其他所有项 | 上表中的默认值 |
| `packages[]` | 只包含通过 `--package` 传入的那个目录 |

版本号脚本会打印 `config_source: inferred`，在 `config_inferred_from:` 里写出推算来源，并在 `config_proposal:` 里输出完整的建议配置。

## 第一次运行：只问一次，再写入

1. 用 JSON 代码块展示建议的配置，并用一句话说明它存的是什么（“记录由哪个工作流发布这个包，以及版本号怎么升”）。
2. 只问一次：要不要保存到 `.agents/release.json`？只问一个“是 / 否”问题。
3. 回答“是”，就原样写入刚刚展示的内容。回答“否”，本次对话继续沿用推算出的值，不再追问。
4. 检查它不会被一起打进要发布的包里：运行 `npm pack --dry-run --ignore-scripts`，看看文件列表里有没有 `.agents/`。如果出现了，说明包的文件列表没有排除它；提议加上或调整 `files`（或者加一行 `.npmignore`），得到同意后再修改。
5. 提醒发布者（Publisher）提交这个文件。不要未经要求就自动提交或推送。

以后修改这个文件也走同样流程：先展示新内容，得到同意后再写入。

## 两个技能分别读什么

| 技能 | 读取的字段 |
| --- | --- |
| `npm-version-guide` | `versioning`、`publish.prereleaseTag`、`packages[]` |
| `npm-publish-guide` | `publish.workflow`、`publish.environment`、`packages[]` |

两个技能各自都能独立使用，不依赖另一个技能，也不要求必须先有这个配置文件。

## 事实来源

- [F-07](../facts.md)
- [F-12](../facts.md)
- [F-85](../facts.md)
- [F-89](../facts.md)
- [F-94](../facts.md)
- [F-102](../facts.md)
- [F-107](../facts.md)

[F-07]: ../facts.md
[F-12]: ../facts.md
[F-85]: ../facts.md
[F-89]: ../facts.md
[F-94]: ../facts.md
[F-102]: ../facts.md
[F-107]: ../facts.md
