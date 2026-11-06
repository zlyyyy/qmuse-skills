# QMuse Agent Skills

Official QMuse CLI skill for AI coding agents. Create, import, iterate, sync, debug, and deploy applications with QMuse.

QMuse 官方 Agent Skill：让编程 Agent 创建应用、导入 React 工程、对话迭代、同步源码、本地云联调并发布版本。

[中文](#中文) · [English](#english) · [QMuse](https://www.qmuse.cn) · [安装指南](https://www.qmuse.cn/skill-setup.md)

## 中文

### 功能

- 创建网页、H5、AI 应用和游戏，也可初始化本地模板工程。
- 导入已有 React 工程，后续修改继续更新同一个应用。
- 通过 QMuse AI 对话持续迭代，查询任务状态并恢复等待输入的任务。
- 拉取平台最新已保存源码，连接真实云资源进行本地开发。
- 查询和管理云资源，发布指定应用版本并查看结果。

### 安装

以下方式需要 Node.js **22.20 或更高版本**，以满足当前 `skills` 安装工具的要求，以及可以登录的 QMuse 账号。

```bash
npm install -g @qmuse/qmuse-cli@latest
npx skills add zlyyyy/qmuse-skills --skill qmuse-cli
```

按安装工具提示选择目标 Agent。默认安装到当前项目；需要全局安装时，给 `npx skills add` 添加 `-g`。

QMuse CLI 自身支持 Node.js 20+；本地云联调需要 Node.js 20.19+。也可通过官方 CLI 安装技能：

```bash
qmuse agent-skill install --agent codex
# 其他工具标识：claude、cursor、trae、codebuddy、qoder、qwen
```

首次操作应用时，按 Agent 提示完成 QMuse 登录和空间选择。更多步骤见[官方安装指南](https://www.qmuse.cn/skill-setup.md)。

### 可以这样告诉 Agent

> 用 QMuse 创建一个带分类的待办应用。

> 把当前 React 工程导入 QMuse，后续修改更新同一个应用。

> 拉取 QMuse 平台最新已保存的代码，继续本地开发。

> 发布这个应用的指定版本，并给我访问链接。

## English

### What this skill does

The `qmuse-cli` skill teaches your coding agent how to use QMuse to:

- Create web, H5, AI, and game applications, or initialize a local template project.
- Import an existing React project and update the same application in later iterations.
- Continue QMuse AI conversations and inspect or resume tasks.
- Pull the latest saved source code and develop locally against real cloud resources.
- Inspect cloud resources and deploy a selected application version.

### Installation

The `skills` installation method above requires **Node.js 22.20+**. Install the companion CLI and the skill:

```bash
npm install -g @qmuse/qmuse-cli@latest
npx skills add zlyyyy/qmuse-skills --skill qmuse-cli
```

Choose your coding agent when prompted. Installation is scoped to the current project by default; add `-g` to install globally.

The QMuse CLI itself requires Node.js 20+, or Node.js 20.19+ for local cloud development. Its built-in installer supports Codex, Claude Code, Cursor, Trae, CodeBuddy, Qoder, and Qwen Code. A QMuse account and sign-in are required for application operations.

### Example prompts

> Use QMuse to create a task manager with categories.

> Import this React project into QMuse and keep updating the same application.

> Pull the latest saved source from QMuse so I can continue development locally.

> Deploy the selected version of this application and return its public URL.

## Skill files

```text
skills/qmuse-cli/
├── SKILL.md
└── references/
    ├── core-flows.md
    └── cloud.md
```

The skill files are copied unchanged from the public npm package `@qmuse/qmuse-cli@1.0.14`. This repository contains the skill and its documentation; install the companion CLI from npm.

- [Skill instructions](skills/qmuse-cli/SKILL.md)
- [Core workflows](skills/qmuse-cli/references/core-flows.md)
- [Cloud services](skills/qmuse-cli/references/cloud.md)
- [QMuse CLI documentation](https://docs.qmuse.cn/docs/qmuse-cli)
- [Skills installer documentation](https://github.com/vercel-labs/skills)

## License

[MIT](LICENSE)
