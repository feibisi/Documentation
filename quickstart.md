# 快速开始

本指南帮助首次访问菲码源库（`feicode.com`）的开发者快速查找公开项目、克隆源码以及下载发行包。

## 1. 无需登录即可使用的功能

菲码源库当前关闭公开自由注册，但所有公开原创仓库与精选镜像均对访客开放以下只读能力：

- 浏览 34 个自有生态组织与公开原创仓库代码、文档及提交记录；
- 通过 HTTPS 协议克隆任意公开仓库；
- 下载公开仓库的 Releases 发行压缩包；
- 通过 `/explore/repos`、`/explore/organizations` 与 `/mirrors` 搜索项目；
- 调用只读公开 API（`https://feicode.com/api/v1/repos/search`）检索公开元数据。

## 2. 查找目标仓库

- **按生态找原创项目**：访问 [探索组织](https://feicode.com/explore/organizations) 查看 `WenPai-org`（文派开源）、`Weixiaoduo`（薇晓朵）、`Windfonts`（文风字体）、`Cyberforums`（赛博论坛）等自有组织。
- **按分类找国内镜像**：访问 [国内开源镜像中心](https://feicode.com/mirrors) 浏览 WordPress、WooCommerce、Moodle、Discourse、开源字体与开发工具镜像。
- **按关键字搜索**：访问 [探索仓库](https://feicode.com/explore/repos) 输入仓库名或简介关键字。

## 3. 使用 HTTPS 克隆公开仓库

确保本机已安装 Git（`git --version`），执行标准克隆命令：

```bash
# 示例：克隆文派原创插件仓库
git clone https://feicode.com/WenPai-org/wpmind.git

# 仅拉取最新一次提交（适合大仓库或 CI 环境）
git clone --depth 1 https://feicode.com/WordPress/WordPress.git
```

## 4. 下载发行包（Releases）

1. 打开目标仓库页面，点击顶部导航栏中的 **发行版（Releases）**；
2. 选择所需版本号，点击对应的 `.zip` 或 `.tar.gz` 附件下载；
3. 浏览器会自动通过官方加速域名 `downloads.feicode.com` 完成下载。

命令行下载示例：

```bash
curl -fLO "https://feicode.com/WenPai-org/wpslug/releases/download/v1.2.1/wpslug.zip"
```

## 5. 参与协作与提交代码

已开通菲码源库账号的开发者，建议继续阅读：

1. [SSH 与 HTTPS 配置](./ssh-and-https.md)：配置 `2222` 端口 SSH 密钥或 HTTPS 访问令牌；
2. [Forgejo Actions 指南](./actions.md)：了解自动化检查与构建工作流；
3. [安全与合规指南](./security.md)：开启 2FA 并遵循分支保护规范。
