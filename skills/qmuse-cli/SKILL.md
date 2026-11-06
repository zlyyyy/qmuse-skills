---
name: qmuse-cli
description: 通过 QMuse 创建、导入、修改、调试、同步或发布应用。用户要求使用 QMuse、操作 qmuse.cn 应用链接，或当前工程已有 QMuse 绑定（.qmuse/project.json 或 .qmuse/app.json）时使用，包括“用 QMuse 做一个应用”“继续修改这个作品”“把本地 React 项目导入 QMuse”及 qmuse 命令排障。普通项目、QMuse 平台或 CLI 自身源码维护沿用仓库研发流程。
---

# QMuse CLI 官方指南

先识别任务：安装与状态查询在本地完成；操作应用时用 `qmuse info --json` 查看身份、空间和绑定，缺少登录再执行 `qmuse login`。已有应用沿用其 appId 和工程；用户给出应用链接时先读取链接中的 appId 并核对应用，避免另建应用。

使用 `qmuse help` 查参数，`--json` 获取结构化结果。导入、对话和发布失败时，读取输出的诊断字段或 `.qmuse/last-import-error.json`；有 jobId、chatId、requestId 时先查原任务，恢复原操作。

## 工程与任务分流（先读这里）

| 用户意图 | 入口 | 关键约束 |
| --- | --- | --- |
| 初始化新的本地 React 工程 | `qmuse create <dir>` | 创建本地模板；开发后用 import 创建远端应用 |
| 将本地 React 工程导入 QMuse | `qmuse import <dir>` | 首次导入创建应用，已有绑定则更新同一应用 |
| 修改已导入的应用 | 修改源码后 `qmuse import .` | 平台基线变化先处理同步冲突，再更新 |
| 用自然语言创建应用 | `qmuse app create <web\|h5\|ai\|game> --file <路径\|->` | web 网页、h5 移动页面、ai AI 应用、game 游戏；返回 appId、chatId 等身份 |
| 继续已有应用的对话 | `qmuse chat send --app-id <id> --chat-id <id> --file <路径\|->` | send 必须传 appId 和 chatId；缺少会话时先查询历史 |
| 回答等待输入的对话 | `qmuse chat resume <chatId> --app-id <id> --interrupt-id <id> --file <回答.json>` | interruptId 来自 chat state 或事件，沿用原会话 |
| 连接真实云资源本地开发 | `qmuse dev [dir]` | 云绑定应用；用 dev status / stop 管理会话 |
| 拉取平台最新已保存源码 | `qmuse pull [dir]` | 本地源码偏离同步基线时停止；成功更新前备份原源码和绑定，再应用远端源码 |
| 单独导出源码 | `qmuse code export --app-id <id> --output <dir>` | 导出最新已保存版；上传使用 import |
| 发布指定版本 | `qmuse deploy --version <id>`、`qmuse release status <releaseId>` | 按用户要求发布；受理后可用 --no-wait 退出，再查结果 |

首次使用身份、导入、对话、同步和发布命令时，按需阅读 [核心流程](references/core-flows.md)；涉及数据库、云函数、配置或密钥时阅读 [云服务指南](references/cloud.md)。

## 技能安装与升级

本指南随 CLI 包离线分发。用 `qmuse agent-skill list` 查看 Codex、Claude Code、Cursor、Trae、CodeBuddy、Qoder 和 Qwen Code 的目录；`qmuse agent-skill install --agent <工具>` 安装到当前工具。自定义安装用 `--target <skills 父目录>`，CLI 自动追加 `qmuse-cli`。

`qmuse upgrade` 更新 CLI 后会同步已安装的默认目录和已登记的自定义目录，保留用户修改。`qmuse agent-skill status --agent <工具> --json` 中 `installed.upToDate` 同时核对版本与文件内容；缺失文件可重新安装修复。更新后重新读取 SKILL.md，必要时刷新工具的技能列表或开启新会话。
