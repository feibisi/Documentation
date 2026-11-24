# 国内开源镜像使用指南

菲码源库为中文开发者维护了一批精选上游开源项目的国内只读镜像，覆盖 WordPress 生态、WooCommerce、Moodle、Discourse、开源字体源文件与常用研发工具，降低跨境拉取波动带来的构建失败风险。

## 一、如何浏览与检索镜像

访问 [国内开源镜像中心（https://feicode.com/mirrors）](https://feicode.com/mirrors)，可按六大分类浏览精选镜像仓库：

1. **WordPress 生态**：WordPress 核心、WP-CLI、Elementor、Avada、BuddyPress、bbPress、MainWP、ClassicPress 等。
2. **WooCommerce 电商**：WooCommerce 核心及常用扩展、物流与支付插件。
3. **Moodle 与在线教育**：Moodle 核心及 H5P 互动教学组件。
4. **Discourse 社区**：Discourse 核心与社区常用插件。
5. **开源字体源文件**：思源黑体、霞鹜文楷、猫啃字库、像素字体等免费可商用开源字体上游仓库。
6. **开发与协作工具**：Forgejo、FreeScout、Mautic、SuiteCRM 与常用 CI Actions。

## 二、使用镜像克隆代码

镜像仓库与普通公开仓库使用相同的 HTTPS 克隆方式：

```bash
# 克隆 WordPress 核心镜像
git clone https://feicode.com/WordPress/WordPress.git

# 浅克隆最新分支以节省带宽与磁盘
git clone --depth 1 https://feicode.com/moodle/moodle.git
```

如果本地仓库原本指向 GitHub，可将其设为只读加速远端，同时保留官方上游用于查看或提 PR：

```bash
git remote add feicode https://feicode.com/WordPress/WordPress.git
git fetch feicode
```

## 三、镜像属性与使用须知

- **只读同步**：镜像仓库定期从上游开源仓库自动拉取，**不接受直接 `git push`**，也不在镜像仓处理上游 Bug 报告或 Pull Request。提交贡献请前往标注的上游官方仓库。
- **版权与协议**：所有镜像均保留上游完整的 Git 历史与开源许可证（LICENSE）。使用镜像代码时，请遵守原项目对应的开源协议。
- **无搜索引擎重复索引**：为尊重上游官方仓库的搜索权重并避免重复内容竞争，外部镜像仓库主页虽可正常访问，但默认输出 `X-Robots-Tag: noindex, follow`，且不包含在 `sitemap-official.xml` 中。
- **深层遍历限制**：为防止自动化爬虫高并发抓取提交树和源码打包接口，镜像仓库的 `/src/`、`/raw/`、`/commits/`、`/blame/` 与 `/archive/` 对匿名请求受限。请始终优先使用 `git clone` 获取源码。
- **不运行 CI**：外部镜像仓库默认关闭 Forgejo Actions。

## 四、申请收录新镜像或反馈同步异常

如发现某个常用开源镜像长时间未同步，或希望推荐新的开源上游项目纳入国内镜像列表，请发送邮件至 `contact@feicode.com`，并在邮件中提供：

1. 上游仓库完整 HTTPS 地址（如 GitHub / Codeberg / GitLab 地址）；
2. 项目开源许可证类型；
3. 简短用途说明。
