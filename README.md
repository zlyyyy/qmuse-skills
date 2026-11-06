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

### 一句话安装

把下面整句话复制给当前使用的编程 Agent：

```text
读取 https://www.qmuse.cn/skill-setup.md 并安装 QMuse CLI 和官方 Skill；已有 qmuse-cli 技能时，用 --target 指向该技能文件夹的父目录并复用原位置，尚未安装时才使用当前 Agent 的默认目录。
```

### 安装

推荐统一使用官方 CLI 安装和升级技能。需要 Node.js **20 或更高版本**、可登录的 QMuse 账号；本地云联调需要 Node.js 20.19+。

```bash
npm install -g @qmuse/qmuse-cli@latest
qmuse agent-skill install --agent codex
qmuse agent-skill status --agent codex --json
```

将 `codex` 换成目标工具标识：`claude`、`cursor`、`trae`、`codebuddy`、`qoder` 或 `qwen`。CLI 使用各工具的用户级技能目录，例如 Codex 是 `~/.codex/skills/qmuse-cli`；安装成功后确认状态中的 `installed.address` 和 `installed.upToDate`。

### 已经通过 GitHub 或技能市场安装

安装位置由安装工具、目标 Agent 和项目/全局范围决定。以 Codex 为例：

| 安装方式 | 目录 |
| --- | --- |
| `qmuse agent-skill install --agent codex` | `~/.codex/skills/qmuse-cli` |
| `npx skills add zlyyyy/qmuse-skills --skill qmuse-cli -a codex` | 当前项目的 `.agents/skills/qmuse-cli` |
| 上述 `npx` 命令加 `-g` | `~/.agents/skills/qmuse-cli` |

同名技能可能装在不同位置。已有本技能时，复用现有位置：将 CLI 的 `--target` 指向已有 `qmuse-cli` 文件夹的**父目录**，安装成功后登记到 CLI 的升级同步中。

```bash
# 例如：已有技能位于当前项目的 .agents/skills/qmuse-cli
qmuse agent-skill install --target ./.agents/skills
qmuse agent-skill status --target ./.agents/skills --json
```

`--target` 会自动追加 `/qmuse-cli`。已选择复用已有目录时，沿用该目录管理；后续使用 `qmuse upgrade` 同步。通过知乎、SkillHub 等平台下载的包也适用同一规则，安装后保留 `SKILL.md` 和 `references/` 的相对路径。

选择 `npx skills` 安装方式时，还需满足其 Node.js 22.20+ 要求。

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

### One-prompt installation

Copy this entire prompt to your coding agent:

```text
Read https://www.qmuse.cn/skill-setup.md and install QMuse CLI and its official skill for the current coding agent. If the qmuse-cli skill is already installed, reuse its existing folder by passing its parent directory to --target; otherwise use the current agent's default skill directory.
```

### Installation

Use the official CLI as the primary installer and updater. It requires **Node.js 20+**, or Node.js 20.19+ for local cloud development, and a QMuse account for application operations:

```bash
npm install -g @qmuse/qmuse-cli@latest
qmuse agent-skill install --agent codex
qmuse agent-skill status --agent codex --json
```

Replace `codex` with `claude`, `cursor`, `trae`, `codebuddy`, `qoder`, or `qwen` for another agent.

### Reuse an existing GitHub or marketplace installation

Matching skill names do not guarantee matching directories. For Codex, the official CLI defaults to `~/.codex/skills/qmuse-cli`, while `npx skills add` uses the project's `.agents/skills/qmuse-cli`, or `~/.agents/skills/qmuse-cli` with `-g`.

If the skill is already installed, point `--target` at its existing **parent directory** so the CLI reuses and registers that location:

```bash
# Existing skill: ./.agents/skills/qmuse-cli
qmuse agent-skill install --target ./.agents/skills
qmuse agent-skill status --target ./.agents/skills --json
```

The CLI appends `/qmuse-cli` to `--target`. Keep using the chosen location, and use `qmuse upgrade` for subsequent synchronization. The same rule applies to packages downloaded from Zhihu, SkillHub, or other marketplaces. Keep `SKILL.md` and `references/` together.

The alternative `npx skills` installer requires Node.js 22.20+.

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
