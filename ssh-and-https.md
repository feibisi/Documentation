# SSH 与 HTTPS 配置指南

菲码源库支持 HTTPS（443 端口）和 SSH（**`2222` 端口**）两种 Git 传输方式。只读克隆公开仓库推荐 HTTPS；日常开发推送代码推荐 SSH 公钥认证。

## 一、配置 SSH 密钥（推荐）

### 1. 生成 Ed25519 密钥

建议为菲码源库生成独立的 Ed25519 密钥对：

```bash
ssh-keygen -t ed25519 -C "your-name@example.com" -f ~/.ssh/id_ed25519_feicode
```

查看公钥内容并复制：

```bash
cat ~/.ssh/id_ed25519_feicode.pub
```

### 2. 在菲码源库登记公钥

1. 登录 [feicode.com](https://feicode.com)；
2. 点击右上角头像，进入 **设置 → SSH / GPG 密钥**；
3. 点击 **增加密钥**，填写易识别的设备名称（如 `macbook-pro-work`），粘贴公钥全文并保存。

### 3. 配置 `~/.ssh/config`

菲码源库的 Git SSH 服务运行在 **`2222` 端口**，在 `~/.ssh/config` 中写入别名可避免每次手动指定端口：

```sshconfig
Host feicode.com
    HostName feicode.com
    Port 2222
    User git
    IdentityFile ~/.ssh/id_ed25519_feicode
    IdentitiesOnly yes
```

### 4. 验证连通性

```bash
ssh -T git@feicode.com
```

看到 `Hi there, <username>! You've successfully authenticated with the key named ...` 即表示配置成功。此后可直接使用：

```bash
git clone ssh://git@feicode.com:2222/WenPai-org/wpmind.git
```

## 二、使用 HTTPS 与个人访问令牌（PAT）

如所在网络环境限制出站 `2222` 端口，可通过 HTTPS 推送代码。开启 2FA 后，HTTPS 认证必须使用个人访问令牌（PAT）替代登录密码。

1. 进入 **设置 → 应用 → 生成新令牌**；
2. 按最小权限原则勾选所需作用域（如仅需读写仓库则只勾选 `write:repository`）；
3. 复制生成的令牌妥善保存（页面关闭后无法再次查看）；
4. 首次执行 `git push` 时，用户名填写菲码账号，密码粘贴令牌，并配合系统凭据管理器缓存：

```bash
# macOS
git config --global credential.helper osxkeychain

# Linux（内存缓存 1 小时）
git config --global credential.helper 'cache --timeout=3600'
```

## 三、Git LFS 大文件支持

部分设计资源、字体源文件或测试数据包仓库启用了 Git LFS。克隆这类仓库前请先安装并初始化 `git-lfs`：

```bash
git lfs install
git clone https://feicode.com/<owner>/<repo>.git
```

若仅需查看文本源码且暂不需要大文件对象，可临时跳过 LFS 下载：

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://feicode.com/<owner>/<repo>.git
```

## 四、常见问题排查

- **`Connection refused`（22 端口）**：未指定 `2222` 端口。请使用 `ssh://git@feicode.com:2222/...` 或检查 `~/.ssh/config` 中的 `Port 2222`。
- **本地代理导致 HTTPS 偶发 `SSL_ERROR_SYSCALL` 或超时**：`feicode.com` 为国内直连节点，建议在代理软件中将 `feicode.com` 加入直连规则，或临时取消代理环境变量测试：
  ```bash
  git -c http.proxy= -c https.proxy= clone https://feicode.com/WenPai-org/wpmind.git
  ```
- **推送被拒绝（`protected branch`）**：目标仓库的 `main` 分支已启用保护规则，请新建特性分支推送并通过 Pull Request 合并。
