# Package_1 AI 安装指南

> 本文档供 AI 阅读，AI 应按照以下步骤完整安装 Package_1。

## 概述

Package_1 是一个个人 AI 助手工具集，包含：
- 7 个 OpenClaw Skills
- 4 个独立工具
- 统一配置管理系统

**位置**: `/home/vimalinx/Projects/MakeMoney/Package_1/`

---

## 安装步骤

### 步骤 1: 检查系统依赖

必须安装以下依赖：

```bash
# 检查是否已安装
node --version        # 需要 Node.js 18+
npm --version
python3 --version     # 需要 Python 3.10+
docker --version
openclaw --version
```

如未安装：
```bash
# 安装 Node.js (如未安装)
# 建议使用 nvm: https://github.com/nvm-sh/nvm

# 安装 OpenClaw (如未安装)
npm install -g openclaw

# 启动 Docker
sudo systemctl start docker
```

### 步骤 2: 查看目录结构

```bash
ls -la /home/vimalinx/Projects/MakeMoney/Package_1/
```

预期结构：
```
Package_1/
├── config/
│   └── config.yaml
├── skills/
│   ├── bocha-search/
│   ├── calendar/
│   ├── task-manager/
│   ├── daily-briefing/
│   ├── feishu-integration/
│   ├── github-helper/
│   └── video-search/
├── tools/
│   ├── banana-slides/
│   ├── browser-use/
│   ├── MediaCrawler/
│   └── 小红书上传/
├── scripts/
│   └── install.sh
├── docs/
│   └── README.md
└── .env.example
```

如果目录不存在或为空，需要先克隆或创建。

### 步骤 3: 安装 Skills

将 skills 复制到 OpenClaw 目录：

```bash
# 创建 OpenClaw skills 目录
mkdir -p ~/.openclaw/skills

# 复制所有 skills
cp -r /home/vimalinx/Projects/MakeMoney/Package_1/skills/* ~/.openclaw/skills/

# 验证
ls ~/.openclaw/skills/
```

预期输出：
```
bocha-search  calendar  daily-briefing  feishu-integration  github-helper  task-manager  video-search
```

### 步骤 4: 配置 API Keys

**方式 A: 编辑 config.yaml**

```bash
nano /home/vimalinx/Projects/MakeMoney/Package_1/config/config.yaml
```

必须配置以下 API Keys：

```yaml
apis:
  bocha:
    api_key: "sk-dc134ccbc935457eb10fe837c8813541"

  Grsai:
    api_key: "sk-72f38f5ad28d463f867e12860aec34ff"
    base_url: "https://grsai.dakka.com.cn/v1beta"
    model: "nano-banana-fast"
```

飞书配置（可选）：
```yaml
feishu:
  app_id: "你的飞书App ID"
  app_secret: "你的飞书App Secret"
```

**方式 B: 设置环境变量**

```bash
export BOCHA_API_KEY="sk-dc134ccbc935457eb10fe837c8813541"
export GRSai_API_KEY="sk-72f38f5ad28d463f867e12860aec34ff"
export FEISHU_APP_ID="cli_xxxxxxxxxxxxx"
export FEISHU_APP_SECRET="你的App Secret"
```

### 步骤 5: 配置 OpenClaw

```bash
# 初始化配置 (如果需要)
openclaw config init

# 设置飞书配置
openclaw config set 'channels.feishu.accounts[0].appId' "你的AppID"
openclaw config set 'channels.feishu.accounts[0].appSecret' "你的AppSecret"

# 启用飞书插件
openclaw plugins enable feishu
```

### 步骤 6: 启动 OpenClaw Gateway

```bash
# 启动服务
openclaw gateway start

# 检查状态
openclaw gateway status

# 或使用 systemctl
systemctl --user start openclaw-gateway.service
systemctl --user status openclaw-gateway.service
```

### 步骤 7: 验证安装

测试 skills 是否生效：

```bash
# 查看已加载的 skills
openclaw tui

# 或查看配置
openclaw config get
```

---

## 各组件说明

### Skills

| Skill | 功能 | 关键配置 |
|-------|------|----------|
| bocha-search | AI 搜索 | BOCHA_API_KEY |
| calendar | 日历管理 | 日历服务账号 |
| task-manager | 任务/GTD/番茄钟 | 本地 JSON 存储 |
| daily-briefing | 每日简报 | 邮件/GitHub API |
| feishu-integration | 飞书文档/知识库 | FEISHU_APP_ID/SECRET |
| github-helper | GitHub 操作 | GITHUB_TOKEN |
| video-search | 视频搜索 | 无需配置 |

### Tools

| Tool | 说明 | 依赖 |
|------|------|------|
| banana-slides | AI PPT 生成 | Docker, Grsai API |
| browser-use | 浏览器自动化 | Playwright |
| MediaCrawler | 社交媒体爬虫 | Python, Redis |
| 小红书上传 | 小红书发布 | Selenium, Cookies |

---

## 故障排查

### Skills 不生效

```bash
# 重启 Gateway
openclaw gateway restart

# 检查 skills 目录
ls ~/.openclaw/skills/

# 查看日志
journalctl --user -u openclaw-gateway.service -f
```

### API 调用失败

```bash
# 测试 API 连接
curl -H "Authorization: Bearer 你的APIKey" API_URL

# 检查环境变量
env | grep -i api
```

### Docker 问题

```bash
# 启动 Docker
sudo systemctl start docker

# 检查容器
docker ps

# 查看日志
docker logs 容器名
```

---

## 快速命令参考

```bash
# 安装 skills
cp -r /home/vimalinx/Projects/MakeMoney/Package_1/skills/* ~/.openclaw/skills/

# 配置飞书
openclaw config set 'channels.feishu.accounts[0].appId' "xxx"
openclaw config set 'channels.feishu.accounts[0].appSecret' "xxx"

# 重启服务
openclaw gateway restart

# 查看状态
openclaw gateway status
```

---

**本文件供 AI 安装使用，按步骤执行即可完成完整安装。**
