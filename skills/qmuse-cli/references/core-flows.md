# 核心流程参考

## 身份与应用

业务命令用 `--json` 获取结构化结果，日志和进度写入 stderr。应用操作前通过 `qmuse info --json` 查看当前身份、空间和绑定。

```bash
qmuse login
qmuse whoami
qmuse space list
qmuse space use <spaceId>
qmuse app create web --file 构思.md --json
qmuse app create h5 --file 移动应用.md --json
qmuse app create ai --file AI应用.md --json
qmuse app create game --file 游戏需求.md --json
qmuse app info <appId>
qmuse app use <appId>
```

创建结果中的 appId、chatId、sprintId 用于后续命令。相同 `--request-id` 重发会恢复已有身份（REUSED），不重新执行首轮生成。生成退出码为 0（完成）、1（失败）、2（等待输入）或 130（取消）；等待输入时查询状态并回答原会话。

## 初始化与导入

```bash
qmuse create my-app
qmuse import /path/to/react-app
qmuse import .                         # 已绑定目录更新同一应用
qmuse import status <jobId> --wait
qmuse import configure <jobId> --cloud-mode EXTERNAL --public-config '@public.json'
```

- 云模式：AUTO（默认）、EXTERNAL（外部云，传公开配置）或 QMUSE（传 `--confirm-cloud-service`）。
- 外部数据不会自动迁移；选择切换时，用 `--acknowledge-empty-cloud-data` 确认。
- 失败时读取 `.qmuse/last-import-error.json` 的阶段、诊断和恢复建议。

## 本地开发与源码同步

```bash
qmuse dev . --port 5173                 # 连接云资源并启动本地 Vite
qmuse dev status
qmuse dev stop
qmuse pull .                           # 拉取平台最新已保存源码
qmuse version list --app-id <id>
qmuse version info <versionId> --app-id <id>
qmuse code export --app-id <id> --output ./exported
```

`pull` 要求本地源码与上次导入或拉取的基线一致；有新增、修改、删除时会停止，Git 提交不会改变同步基线。成功更新前会将原源码和绑定存入返回的 backupDirectory，再应用远端源码。上传改动用 `import`；下载用 `pull` 或 `code export`。未保存的远端对话改动不会被拉取。

CLI 登录凭证留在本地凭证存储，不放入业务源码或浏览器配置。

## 对话与恢复

```bash
qmuse chat history --app-id <appId> --sprint-id <sprintId>
qmuse chat send --app-id <appId> --chat-id <chatId> --file 需求.md --json
qmuse chat state <chatId>
qmuse chat resume <chatId> --app-id <appId> --interrupt-id <interruptId> --file 回答.json --json
qmuse chat attachment upload <path> --app-id <appId>
qmuse models
```

send 的 appId 和 chatId 必须显式传入；history 的 appId 和 sprintId 也必须显式传入。断流后先查 state 和 history；等待输入时，沿用原 chatId 和返回的 interruptId，用 JSON 文件回答。需要流式结构化事件时用 `--ndjson`。

## 云数据

```bash
qmuse data-source list --app-id <id>
qmuse db tables --app-id <id>
qmuse db records list --app-id <id> --form-id <formId>
qmuse db permission get --app-id <id> --form-id <formId>
```

表列表不需要 formId；记录读写和表权限查询需要 formId。云架构、函数和密钥命令见 [云服务指南](cloud.md)。

## 发布与恢复

```bash
qmuse deploy --version <versionId> --no-wait
qmuse release status <releaseId> --wait
qmuse release list --app-id <appId>
qmuse account
```

按用户指定版本发布，结果以 release status 返回的状态、检查结果和地址为准；受理不等于发布成功。失败先查原发布任务的错误，再按诊断恢复。
