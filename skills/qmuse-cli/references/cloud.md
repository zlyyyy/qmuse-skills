# 云服务指南

CLI 不复制云规则，只做两件事：识别当前应用的实际架构，然后把操作路由到现有权威入口。

## 1. 识别当前架构

```bash
qmuse info --json          # bindings + cloud 汇总（entitlement / environment）
qmuse cloud status --app-id <appId> --json   # effectiveMode、activationStatus、publishAllowed、canRetryInitialization
```

- `SINGLE_ENV` / Legacy：应用没有开通多环境；云初始化与首版同步由导入或对话侧完成。
- `MULTI_ENV`（Hono）：`qmuse cloud enable` 之后再查询状态；已初始化失败用 `qmuse cloud environment retry --wait`。

## 2. 路由到现有入口

| 判断 | 动作 |
| --- | --- |
| 权益预检（不伪装已开通） | `qmuse cloud init -d "<功能描述>"` |
| 实际开通 | `qmuse cloud enable --app-id <appId>` |
| 状态/失败原因 | `qmuse cloud status --app-id <appId>` |
| 初始化失败重试 | `qmuse cloud environment retry --app-id <appId> --wait` |
| 公开连接与运行配置 | `qmuse cloud pull-env --app-id <appId> --output .qmuse/local-dev.json` |
| 数据库/表结构与用量 | `qmuse data-source list\|info`、`qmuse db tables`、`qmuse db records …` |
| 云函数 | `qmuse fn list\|info\|retry\|deployment-*\|execution-*` |
| 秘密（安全输入） | `qmuse cloud set-secret K1,K2 --stdin`；查询 `qmuse secret list` / `secret sync status\|retry` |

## 3. 约束

- 管理凭证只保留在 CLI 会话或服务端；`pull-env` 输出仅含公开配置。
- 预检通过不等于初始化完成：以 `cloud status` 的 `activationStatus` 为准。
- 套餐与订阅购买流程在 PC 端；CLI 只读 `qmuse account` 汇总并在缺少契约时返回 `unavailable` 原因。
