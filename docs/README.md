# Package_1 工具集

> 个人 AI 助手工具集合，包含 OpenClaw Skills 和独立工具。

## 📁 目录结构

```
Package_1/
├── config/
│   └── config.yaml          # 统一配置文件
├── skills/                  # OpenClaw Skills
│   ├── bocha-search/        # 博查 AI 搜索
│   ├── calendar/            # 日历管理
│   ├── task-manager/        # 任务管理
│   ├── daily-briefing/     # 每日简报
│   ├── feishu-integration/ # 飞书集成
│   ├── github-helper/       # GitHub 辅助
│   └── video-search/       # 视频搜索
├── tools/                   # 独立工具
│   ├── browser-use/        # 浏览器自动化
│   ├── MediaCrawler/       # 小红书/抖音爬虫
│   ├── 小红书上传/         # 小红书图片上传
│   └── banana-slides/     # AI PPT 生成
├── scripts/                 # 脚本
│   └── install.sh         # 安装脚本
├── docs/                    # 文档
└── .env.example            # 环境变量模板
```

## 🚀 快速开始

### 1. 安装依赖

```bash
# 安装 OpenClaw (如果未安装)
npm install -g openclaw

# 进入目录
cd Package_1

# 运行安装脚本
chmod +x scripts/install.sh
./scripts/install.sh
```

### 2. 配置

编辑 `config/config.yaml`，填入你的 API Keys：

```yaml
apis:
  bocha:
    api_key: "你的博查API Key"
  Grsai:
    api_key: "你的Grsai API Key"
```

### 3. 使用 Skills

将 skills 复制到 OpenClaw 目录：

```bash
cp -r skills/* ~/.openclaw/skills/
```

### 4. 启动服务

```bash
openclaw gateway start
```

## 📦 Skills 列表

| Skill | 功能 | 文档 |
|-------|------|------|
| bocha-search | 博查 AI 搜索 | [SKILL.md](skills/bocha-search/SKILL.md) |
| calendar | 日历管理 | [SKILL.md](skills/calendar/SKILL.md) |
| task-manager | 任务管理 | [SKILL.md](skills/task-manager/SKILL.md) |
| daily-briefing | 每日简报 | [SKILL.md](skills/daily-briefing/SKILL.md) |
| feishu-integration | 飞书集成 | [SKILL.md](skills/feishu-integration/SKILL.md) |
| github-helper | GitHub 辅助 | [SKILL.md](skills/github-helper/SKILL.md) |
| video-search | 视频搜索 | [SKILL.md](skills/video-search/SKILL.md) |
| xhs-hot-tracker | 小红书热点追踪 | [SKILL.md](skills/xhs-hot-tracker/SKILL.md) |
| xhs-ai-writer | 小红书 AI 写作 | [SKILL.md](skills/xhs-ai-writer/SKILL.md) |
| xhs-publisher | 小红书自动发布 | [SKILL.md](skills/xhs-publisher/SKILL.md) |
| xhs-data-tracker | 小红书数据追踪 | [SKILL.md](skills/xhs-data-tracker/SKILL.md) |
| xhs-image-generator | 小红书图片处理 | [SKILL.md](skills/xhs-image-generator/SKILL.md) |
| xhs-scheduler | 小红书定时任务 | [SKILL.md](skills/xhs-scheduler/SKILL.md) |
| xhs-virus-analyzer | 小红书爆款分析 | [SKILL.md](skills/xhs-virus-analyzer/SKILL.md) |
| xhs-analytics | 小红书数据分析 | [SKILL.md](skills/xhs-analytics/SKILL.md) |
| xhs-content-finder | 小红书素材搜索 | [SKILL.md](skills/xhs-content-finder/SKILL.md) |

## 🔧 工具列表

| 工具 | 功能 | 说明 |
|------|------|------|
| browser-use | 浏览器自动化 | AI 驱动的浏览器控制 |
| MediaCrawler | 社交媒体爬虫 | 小红书、抖音数据采集 |
| 小红书上传 | 图片发布 | 自动上传图片到小红书 |
| banana-slides | PPT 生成 | AI 生成演示文稿 (暂不可用) |

## ⚙️ 配置说明

### config.yaml 字段说明

| 字段 | 说明 | 示例 |
|------|------|------|
| `apis.bocha.api_key` | 博查搜索 API Key | 在 bocha.ai 申请 |
| `apis.Grsai.api_key` | Grsai API Key | 用于 PPT 生成 |
| `feishu.app_id` | 飞书应用 ID | cli_xxx |
| `feishu.app_secret` | 飞书应用密钥 | 在开放平台获取 |

### 环境变量

也可以通过环境变量覆盖配置：

```bash
export BOCHA_API_KEY="sk-xxx"
export FEISHU_APP_ID="cli_xxx"
export FEISHU_APP_SECRET="xxx"
```

## 📋 系统要求

- **Node.js**: 18+
- **Python**: 3.10+ (部分工具需要)
- **Docker**: 最新版 (工具需要)
- **OpenClaw**: 最新版

## 🆘 故障排查

### Skills 不生效？

```bash
# 复制 skills 到 OpenClaw 目录
cp -r skills/* ~/.openclaw/skills/

# 重启 Gateway
openclaw gateway restart
```

### API 调用失败？

```bash
# 检查 API Key 配置
cat config/config.yaml | grep api_key

# 测试 API 连接
curl -H "Authorization: Bearer YOUR_API_KEY" API_URL
```

## 📄 许可证

MIT License

---

**维护者**: Wilson (AI Assistant)  
**版本**: 1.0.0  
**更新日期**: 2026-02-13
