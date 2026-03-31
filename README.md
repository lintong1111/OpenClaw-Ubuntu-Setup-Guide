# Ubuntu 虚拟机安装 OpenClaw 完整流程

本教程适用于在 Ubuntu 虚拟机环境下安装 OpenClaw。

## 官方安装方式（推荐）

OpenClaw 提供一键安装脚本，自动检测系统、安装 Node.js、部署 OpenClaw 并启动引导。

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

安装完成后会自动启动引导流程，按提示操作即可。

### 不运行引导（静默安装）

```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

## 系统要求

- **Node.js 24**（推荐）或 Node.js 22.14+
- **系统**：Ubuntu 20.04+（Linux / WSL2 均支持）
- 安装脚本会自动安装 Node.js（如果缺失）

## 前提条件

### 1. 开启 sudo 免密（可选但推荐）

避免安装过程中频繁输入密码：

```bash
sudo visudo
```

在文件末尾添加（将 `用户名` 替换为实际用户名）：

```
用户名 ALL=(ALL) NOPASSWD: ALL
```

保存退出：`Ctrl+O` → `Enter` → `Ctrl+X`

### 2. 安装必要工具

```bash
sudo apt update
sudo apt install -y curl git
```

### 3. 开启 SSH（远程管理用）

```bash
sudo apt install -y openssh-server
sudo systemctl start ssh
sudo systemctl enable ssh
```

查看虚拟机 IP：

```bash
ip addr show | grep -E "inet " | awk '{print $2}' | cut -d'/' -f1 | grep -v "^127"
```

记录此 IP，后续可通过 SSH 远程管理。

## 完整安装步骤

### Step 1：下载并运行安装脚本

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

安装脚本会自动：
- 检测操作系统（Ubuntu）
- 安装 Node.js（如缺失）
- 安装 OpenClaw
- 启动引导流程

### Step 2：按引导完成配置

安装完成后，终端会显示引导链接和验证码，按提示操作即可。

### Step 3：验证安装

```bash
openclaw status
```

### Step 4：配置国内 API（如需要）

参考：[OpenClaw 国内 API 配置指南](https://github.com/lintong1111/OpenClaw-CN-API-Guide)

## 其他安装方式

### npm 安装

```bash
# 安装 Node.js（如果未安装）
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard

# 使用 npm 全局安装
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

### pnpm 安装

```bash
pnpm add -g openclaw@latest
pnpm approve-builds -g
openclaw onboard --install-daemon
```

## 常见问题

### Q: 安装脚本权限被拒绝？

```bash
chmod +x install.sh
./install.sh
```

### Q: Node.js 版本不对？

安装脚本会自动处理。如需手动安装：

```bash
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs
```

### Q: 引导验证码在哪里？

运行 `openclaw status` 或查看 `~/.openclaw/gateway.json`。

### Q: 如何卸载？

```bash
npm uninstall -g openclaw
# 或参考官方文档
```

## 相关仓库

- [OpenClaw 国内 API 配置指南](https://github.com/lintong1111/OpenClaw-CN-API-Guide)
- [OpenClaw 官方文档](https://docs.openclaw.ai)

## 许可证

MIT License
