# dev-setup-cn

> **一站式中国开发者环境配置工具** — 极速切换国内镜像源，告别网络延迟

[![GitHub stars](https://img.shields.io/github/stars/rayyee/dev-setup-cn?style=social)](https://github.com/rayyee/dev-setup-cn)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Linux%20%7C%20Windows%20(Git--Bash)-lightgrey)](https://github.com/rayyee/dev-setup-cn)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/rayyee/dev-setup-cn/pulls)

---

## 🚀 秒速解决这些痛点

- `npm install` 卡死 → **腾讯云镜像**
- `pip install` 超时 → **阿里云镜像**
- Homebrew 龟速安装 → **清华 TUNA 镜像**
- Docker pull 等到天亮 → **DaoCloud + DockerProxy**
- Maven 依赖下载如蜗牛 → **阿里云 Maven 镜像**
- Go modules 拉取失败 → **goproxy.cn**
- Composer 慢如老牛 → **阿里云 Composer 镜像**
- RubyGems 安装无响应 → **Ruby China 镜像**
- Conda 包下载中断 → **清华 Conda 镜像**

**一个命令，全盘搞定。**

---

## 📦 一键安装

```bash
curl -fsSL https://raw.githubusercontent.com/rayyee/dev-setup-cn/main/install.sh | bash
```

脚本自动检测您的操作系统（**macOS / Linux / Windows Git-Bash**），无需额外配置。

> **Windows 用户**：请使用 [Git for Windows](https://git-scm.com/download/win) 自带的 **Git-Bash** 运行此脚本。

---

## ✨ 核心特性

- **全平台支持**：macOS、Linux、Windows (Git-Bash) 一网打尽  
- **模块化配置**：可单独配置任一工具，也可一键全量  
- **智能检测**：自动跳过未安装的工具，无干扰  
- **零配置启动**：开箱即用，无需提前安装依赖  
- **持续扩展**：轻松添加新镜像源（见下方扩展指南）

---

## 📋 包含的镜像源

| 工具               | 镜像源                              | 支持平台         |
|--------------------|-------------------------------------|------------------|
| npm                | 腾讯云 `mirrors.cloud.tencent.com`  | 全平台           |
| pip                | 阿里云 `mirrors.aliyun.com`         | 全平台           |
| Maven              | 阿里云 `maven.aliyun.com`           | 全平台           |
| Go (GOPROXY)       | `goproxy.cn`                        | 全平台           |
| Docker             | DaoCloud + DockerProxy              | 全平台           |
| Homebrew           | 清华 TUNA                           | macOS 专属       |
| Composer (PHP)     | 阿里云 `mirrors.aliyun.com/composer`| 全平台           |
| RubyGems           | Ruby China `gems.ruby-china.com`    | 全平台           |
| Conda              | 清华 TUNA (Anaconda)                | 全平台           |
| Git 基础配置       | `main` 分支、`autocrlf`、`pull.rebase` | 全平台         |

---

## 🛠 使用方法

### 完整配置（推荐）

```bash
./dev-setup-cn.sh
```

会依次配置 **Git + 上述所有镜像源**（仅当对应工具已安装时）。

### 分组配置

| 命令                               | 作用                               |
|------------------------------------|------------------------------------|
| `./dev-setup-cn.sh --mirrors-only` | 配置所有镜像源（不含 Git）          |
| `./dev-setup-cn.sh --frontend`     | 仅配置 npm 镜像                    |
| `./dev-setup-cn.sh --java`         | 仅配置 Maven 镜像                  |
| `./dev-setup-cn.sh --git`          | 仅配置 Git 基础设置                |

### 精细控制（单工具模式）

您可以通过 `--<tool>` 精确指定要配置的工具，支持**任意组合**：

```bash
# 只配置 npm 和 Docker
./dev-setup-cn.sh --npm --docker

# 只配置 Gem 和 Conda
./dev-setup-cn.sh --gem --conda

# 只配置 Homebrew (仅 macOS)
./dev-setup-cn.sh --homebrew

# 同时配置 Git、Maven、Go
./dev-setup-cn.sh --git --maven --go
```

> **注意**：一旦使用任何 `--<tool>` 参数，分组参数（`--mirrors-only` 等）将被忽略，仅执行您指定的工具。

### 可用单工具参数清单

| 参数            | 对应工具       |
|-----------------|----------------|
| `--npm`         | npm            |
| `--pip`         | pip            |
| `--maven`       | Maven          |
| `--go`          | Go             |
| `--docker`      | Docker         |
| `--homebrew`    | Homebrew (macOS)|
| `--composer`    | Composer (PHP) |
| `--gem`         | RubyGems       |
| `--conda`       | Conda          |
| `--git`         | Git 基础配置   |

---

## 🔧 手动配置参考

如果您更愿意手动操作，以下为各工具的配置命令：

<details>
<summary><b>点击展开</b></summary>

### npm 镜像（腾讯云）

```bash
npm config set registry https://mirrors.cloud.tencent.com/npm/
```

### pip 镜像（阿里云）

```bash
mkdir -p ~/.pip
cat > ~/.pip/pip.conf << 'EOF'
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
timeout = 120
EOF
```

### Go 代理（goproxy.cn）

```bash
go env -w GOPROXY=https://goproxy.cn,direct
go env -w GONOSUMDB=*
```

### Maven 镜像（阿里云）

在 `~/.m2/settings.xml` 中添加：

```xml
<mirrors>
  <mirror>
    <id>aliyun</id>
    <name>阿里云 Maven 镜像</name>
    <mirrorOf>*</mirrorOf>
    <url>https://maven.aliyun.com/repository/public</url>
  </mirror>
</mirrors>
```

### Docker 镜像（DaoCloud + DockerProxy）

配置文件位置：
- macOS / Windows: `~/.docker/daemon.json`
- Linux: `/etc/docker/daemon.json`

```json
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://dockerproxy.com"
  ]
}
```

### Homebrew 镜像（清华 TUNA）

```bash
export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
```

### Composer 镜像（阿里云）

```bash
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

### RubyGems 镜像（Ruby China）

```bash
gem sources --remove https://rubygems.org/
gem sources --add https://gems.ruby-china.com/
```

### Conda 镜像（清华 TUNA）

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --set show_channel_urls yes
```

### Git 基础配置

```bash
git config --global init.defaultBranch main
git config --global core.autocrlf input
git config --global pull.rebase false
```

</details>

---

## 🤝 如何扩展

若您想为脚本添加新的镜像源或工具，只需：

1. 在脚本中编写对应的配置函数（例如 `setup_yarn_mirror`）。
2. 在 `TOOL_MAP` 关联数组中新增一行：
   ```bash
   [yarn]="setup_yarn_mirror"
   ```
3. 用户即可使用 `--yarn` 参数调用。

无需修改任何 `case` 语句或变量声明，极大降低维护成本。

---

## 📄 许可证

本项目基于 **MIT License** 开源。

---

## 🙏 致谢与来源

本项目最初由 [huanglei288766](https://github.com/huanglei288766) 创建，旨在解决国内开发者的网络环境配置难题。  
现由 [rayyee](https://github.com/rayyee) 维护增强，扩展了 Windows 支持、更多镜像源及精细控制能力。

感谢所有贡献者和镜像服务提供方（阿里云、腾讯云、清华大学 TUNA、DaoCloud、goproxy.cn、Ruby China 等）的卓越支持。

---

**立即开始**：`curl -fsSL https://raw.githubusercontent.com/rayyee/dev-setup-cn/main/install.sh | bash`

---