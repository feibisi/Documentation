# Forgejo Actions 使用指南

菲码源库启用了 Forgejo Actions，用于执行自有原创仓库的代码静态检查、单元测试、制品打包与受控发布。其工作流语法与 GitHub Actions 高度兼容。

## 一、目录结构与基本示例

在仓库根目录创建 `.forgejo/workflows/` 目录，放入 YAML 工作流文件（例如 `.forgejo/workflows/ci.yml`）：

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: https://feicode.com/actions/checkout@v4

      - name: Run checks
        run: |
          echo "Running tests on FeiCode Actions"
```

> 提示：优先使用菲码源库已镜像的本地 Action（如 `https://feicode.com/actions/checkout@v4`、`https://feicode.com/actions/setup-go@v5`），避免跨境拉取外网 Action 失败。

## 二、权限范围与策略

- **自有原创仓库**：可按需在仓库 **设置 → 仓库单元** 中开启 Actions，并关联集群托管的专用 Runner 执行构建。
- **外部镜像仓库**：所有外部拉取镜像默认关闭 Actions，防止上游每次同步自动触发无意义或不可信的第三方流水线。
- **公共 Runner 隔离**：严禁在不受信任的 Pull Request 上直接向构建环境注入生产发布凭据。

## 三、Secrets 与变量管理

1. 敏感凭据（API Token、签名私钥、发布凭据等）必须存放在仓库或组织的 **设置 → Actions → 密钥（Secrets）** 中，禁止明文提交进仓库代码或打印到工作流日志。
2. 非敏感配置（如目标环境名称、版本号前缀）可使用 **变量（Variables）** 存放。
3. 在工作流中通过 `${{ secrets.MY_TOKEN }}` 引用密钥，日志输出会自动脱敏掩码。

## 四、编写高效工作流的建议

- **设置超时**：为每个 `job` 显式配置 `timeout-minutes`（例如 `timeout-minutes: 15`），避免异常挂起长期占用 Runner。
- **按路径过滤**：纯文档改动无需运行完整编译测试，可通过 `paths` / `paths-ignore` 精准触发。
- **控制并发**：在 Pull Request 频繁推送时启用 `concurrency` 自动取消旧的未完成运行：

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

- **大制品走发行通道**：构建产物如需长期公开下载，请归档至仓库 Releases（由 `downloads.feicode.com` 分发），不要长期堆积在临时 Actions Artifacts 中。
