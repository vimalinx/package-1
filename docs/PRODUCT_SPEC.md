# 小红书自动化运营工具 - 产品需求文档

> 本文档供 AI 编程使用，详细描述需要开发的功能和技术要求。

---

## 1. 项目概述

### 1.1 目标

构建一个以 **OpenClaw** 为核心的小红书自动化运营工具，实现：
- ✅ 自动跟踪市场热点
- ✅ 抓取爆款内容
- ✅ AI 辅助创作笔记
- ✅ 自动发布笔记
- ✅ 数据追踪分析

### 1.2 核心流程

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  热点追踪   │ -> │  内容分析   │ -> │  AI 创作   │ -> │  自动发布   │ -> │  数据追踪   │
│  (爬虫)     │    │  (数据分析) │    │  (文案/图片)│    │  (发布笔记) │    │  (效果统计) │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

---

## 2. 系统架构

### 2.1 整体架构

```
┌─────────────────────────────────────────────────────────────────────┐
│                         OpenClaw Gateway                             │
│                    (统一调度、消息路由、Skill 管理)                   │
└─────────────────────────────────────────────────────────────────────┘
         ↑                ↑                ↑                ↑
    ┌────┴────┐      ┌────┴────┐      ┌────┴────┐      ┌────┴────┐
    │ Skills  │      │ Skills  │      │ Skills  │      │ Skills  │
    │         │      │         │      │         │      │         │
    │热点追踪  │      │ AI 写作 │      │ 定时任务 │      │ 数据分析│
    └─────────┘      └─────────┘      └─────────┘      └─────────┘
         ↑                ↑                ↑                ↑
    ┌────┴────────────────┴────────────────┴────────────────┴────┐
    │                         外部服务                              │
    │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────────┐   │
    │  │ 小红书  │  │  Grsai  │  │  BoCha  │  │  SQLite    │   │
    │  │  API    │  │  AI     │  │  搜索   │  │  数据库    │   │
    │  └─────────┘  └─────────┘  └─────────┘  └─────────────┘   │
    └──────────────────────────────────────────────────────────────┘
```

### 2.2 技术栈

| 层级 | 技术 | 说明 |
|------|------|------|
| **调度层** | OpenClaw | 消息路由、Skill 管理、定时任务 |
| **爬虫层** | Playwright / Requests | 网页抓取、模拟登录 |
| **AI 层** | Grsai API (nano-banana-fast) | 文案生成、标题生成 |
| **搜索层** | BoCha API | 热点搜索、趋势分析 |
| **存储层** | SQLite | 结构化数据存储 |
| **发布层** | 浏览器自动化 | 小红书发布 |

---

## 3. 功能模块

### 3.1 热点追踪模块 (Hot Tracker)

**功能**：自动追踪小红书热门内容

#### 3.1.1 热门榜单爬虫

```python
# skill: xhs-hot-tracker
# 功能：爬取小红书热门笔记
# 接口：
class HotTracker:
    def get_hot_notes(category: str, limit: int = 50) -> List[Dict]:
        """
        获取热门笔记
        category: 分类 (美食/美妆/穿搭/旅行/数码/健身等)
        limit: 返回数量
        返回: [{id, title, author, likes,收藏, comments, tags, images}]
        """
        
    def get_hot_topics() -> List[Dict]:
        """
        获取热门话题
        返回: [{topic_id, name, heat, related_notes}]
        """
        
    def get_trending_keywords() -> List[str]:
        """
        获取上升关键词
        返回: ["近期爆款关键词"]
        """
```

#### 3.1.2 爆款分析

```python
# skill: xhs-virus-analyzer
# 功能：分析爆款规律
class VirusAnalyzer:
    def analyze_content(note_id: str) -> Dict:
        """
        分析单篇爆款
        返回: {爆点分析, 标题特点, 内容结构, 标签建议}
        """
        
    def get_content_patterns(category: str, min_likes: int) -> List[Dict]:
        """
        获取内容模式
        返回: [{pattern_type, examples, tips}]
        """
```

### 3.2 AI 创作模块 (AI Creator)

**功能**：AI 辅助生成笔记文案

#### 3.2.1 文案生成

```python
# skill: xhs-ai-writer
# 功能：生成小红书笔记文案
class XHSWriter:
    def generate_title(topic: str, style: str = "吸引眼球") -> str:
        """
        生成标题
        topic: 主题
        style: 风格 (种草/干货/测评/故事)
        返回: "生成的标题"
        """
        
    def generate_content(
        topic: str,
        keywords: List[str],
        style: str = "种草",
        length: str = "medium"  # short/medium/long
    ) -> str:
        """
        生成正文
        返回: "生成的文案"
        """
        
    def generate_tags(topic: str, count: int = 5) -> List[str]:
        """
        生成标签
        返回: ["#标签1", "#标签2", ...]
        """
        
    def generate_full_note(
        topic: str,
        category: str,
        reference_notes: List[Dict] = None
    ) -> Dict:
        """
        生成完整笔记
        返回: {title, content, tags, images_suggestions}
        """
```

#### 3.2.2 图片生成/处理

```python
# skill: xhs-image-generator
# 功能：生成/处理笔记图片
class ImageGenerator:
    def generate_cover(topic: str, style: str) -> str:
        """
        生成封面图
        返回: 图片路径
        """
        
    def add_watermark(image_path: str, text: str) -> str:
        """
        添加水印
        返回: 处理后的图片路径
        """
        
    def create_collage(images: List[str], layout: str) -> str:
        """
        拼接图片
        layout: 布局 (grid/timeline)
        返回: 拼接后的图片路径
        """
        
    def add_text_overlay(image_path: str, text: str, position: str) -> str:
        """
        添加文字
        返回: 处理后的图片路径
        """
```

### 3.3 自动发布模块 (Auto Publisher)

**功能**：自动发布笔记到小红书

#### 3.3.1 笔记发布

```python
# skill: xhs-publisher
# 功能：自动发布笔记
class XHSPublisher:
    def login(cookies: str) -> bool:
        """
        登录小红书
        cookies: 小红书 cookies
        返回: 登录是否成功
        """
        
    def publish_note(
        title: str,
        content: str,
        images: List[str],
        tags: List[str],
        location: str = None
    ) -> Dict:
        """
        发布笔记
        返回: {success, note_id, url}
        """
        
    def upload_image(image_path: str) -> str:
        """
        上传图片
        返回: 上传后的图片 ID
        """
```

#### 3.3.2 定时发布

```python
# skill: xhs-scheduler
# 功能：定时发布任务
class XHSScheduler:
    def schedule_note(
        note: Dict,
        publish_time: datetime
    ) -> str:
        """
        定时发布
        返回: 任务 ID
        """
        
    def cancel_scheduled(task_id: str) -> bool:
        """
        取消定时任务
        """
        
    def list_scheduled() -> List[Dict]:
        """
        查看待发布任务
        """
```

### 3.4 数据追踪模块 (Data Tracker)

**功能**：追踪笔记发布后的数据

#### 3.4.1 数据采集

```python
# skill: xhs-data-tracker
# 功能：追踪笔记数据
class DataTracker:
    def get_note_stats(note_id: str) -> Dict:
        """
        获取笔记数据
        返回: {views, likes,收藏, comments, shares}
        """
        
    def track_note(note_id: str, interval: int = 3600):
        """
        持续追踪笔记
        interval: 追踪间隔(秒)
        """
        
    def get_profile_stats() -> Dict:
        """
        获取账号数据
        返回: {粉丝数, 获赞数, 笔记数}
        """
```

#### 3.4.2 数据分析

```python
# skill: xhs-analytics
# 功能：数据分析报表
class Analytics:
    def get_performance_report(days: int = 7) -> Dict:
        """
        效果报表
        返回: {总曝光, 总点赞, 最佳发布时间, 最佳话题}
        """
        
    def get_content_report() -> Dict:
        """
        内容分析
        返回: {爆款率, 平均互动, 话题分布}
        """
```

### 3.5 搜索素材模块 (Content Finder)

**功能**：搜索参考内容和素材

#### 3.5.1 热点搜索

```python
# skill: xhs-content-finder
# 功能：搜索参考内容
class ContentFinder:
    def search_notes(keyword: str, category: str = None, sort: str = "hot") -> List[Dict]:
        """
        搜索笔记
        keyword: 关键词
        category: 分类
        sort: 排序 (hot/latest)
        返回: [{id, title, author, likes, ...}]
        """
        
    def search_images(keyword: str, count: int = 10) -> List[str]:
        """
        搜索图片
        返回: 图片 URL 列表
        """
```

---

## 4. 数据模型

### 4.1 数据库设计 (SQLite)

```sql
-- 热门笔记表
CREATE TABLE hot_notes (
    id INTEGER PRIMARY KEY,
    note_id TEXT UNIQUE,
    title TEXT,
    author TEXT,
    category TEXT,
    likes INTEGER,
   收藏 INTEGER,
    comments INTEGER,
    tags TEXT,
    image_urls TEXT,
    crawled_at DATETIME,
    analysis_data TEXT
);

-- 发布的笔记表
CREATE TABLE published_notes (
    id INTEGER PRIMARY KEY,
    note_id TEXT UNIQUE,
    title TEXT,
    content TEXT,
    tags TEXT,
    images TEXT,
    published_at DATETIME,
    status TEXT DEFAULT 'published',
    xhs_url TEXT
);

-- 笔记数据追踪表
CREATE TABLE note_stats (
    id INTEGER PRIMARY KEY,
    note_id TEXT,
    recorded_at DATETIME,
    views INTEGER DEFAULT 0,
    likes INTEGER DEFAULT 0,
   收藏 INTEGER DEFAULT 0,
    comments INTEGER DEFAULT 0,
    shares INTEGER DEFAULT 0
);

-- 定时任务表
CREATE TABLE scheduled_tasks (
    id INTEGER PRIMARY KEY,
    title TEXT,
    content TEXT,
    images TEXT,
    tags TEXT,
    scheduled_at DATETIME,
    status TEXT DEFAULT 'pending',
    created_at DATETIME
);

-- 关键词追踪表
CREATE TABLE tracked_keywords (
    id INTEGER PRIMARY KEY,
    keyword TEXT UNIQUE,
    category TEXT,
    priority INTEGER DEFAULT 0,
    last_tracked DATETIME
);
```

---

## 5. OpenClaw Skill 标准格式

每个模块需要实现为 OpenClaw Skill，格式如下：

```
skills/
└── xhs-hot-tracker/
    ├── SKILL.md          # 技能说明文档
    ├── skill.yaml        # 技能配置
    └── scripts/
        └── main.sh       # 主执行脚本
```

### SKILL.md 模板

```markdown
name: xhs-hot-tracker
description: 小红书热点追踪 - 实时抓取热门笔记、话题、关键词

---

# 功能

- get_hot_notes: 获取热门笔记
- get_hot_topics: 获取热门话题
- get_trending_keywords: 获取上升关键词

# 使用

```bash
# 获取今日热门
xhs-hot get notes --category 美食 --limit 20

# 获取热门话题
xhs-hot get topics

# 获取上升关键词
xhs-hot get keywords
```

# 配置

需要配置：
- 小红书 cookies (用于登录爬取)
- 爬取间隔
- 分类列表
```

---

## 6. 定时任务配置

### 6.1 热点追踪任务

```yaml
# cron 配置
jobs:
  - name: xhs-hot-tracker
    schedule: "0 * * * *"  # 每小时执行
    action: xhs-hot-tracker.run
    params:
      categories: [美食, 美妆, 穿搭, 旅行, 数码]
      limit: 50
      
  - name: xhs-keyword-tracker
    schedule: "0 6,12,18 * * *"  # 每天 3 次
    action: xhs-hot-tracker.keywords
```

### 6.2 数据追踪任务

```yaml
  - name: xhs-stats-collector
    schedule: "0 */4 * * *"  # 每 4 小时
    action: xhs-data-tracker.collect_all
    
  - name: xhs-report-generator
    schedule: "0 8 * * 1"  # 每周一早上
    action: xhs-analytics.weekly_report
```

---

## 7. 配置文件

### config.yaml 扩展

```yaml
# 小红书配置
xiaohongshu:
  enabled: true
  cookies: ""  # 你的小红书 cookies
  # 爬取配置
  crawler:
    categories:
      - 美食
      - 美妆
      - 穿搭
      - 旅行
      - 数码
      - 健身
      - 家居
      - 职场
    interval: 3600  # 爬取间隔(秒)
    min_likes: 1000  # 最小热度
    
  # 发布配置
  publisher:
    default_tags:
      - "#种草"
      - "#好物推荐"
    auto_add_location: true
    
  # 数据追踪
  tracker:
    enabled: true
    track_interval: 3600  # 追踪间隔
    
# AI 写作配置
ai_writer:
  enabled: true
  provider: "grsai"  # 或 openai/gemini
  api_key: ""
  model: "nano-banana-fast"
  # 写作风格
  styles:
    - name: "种草"
      prompt: "你是一个小红书种草达人..."
    - name: "干货"
      prompt: "你是一个专业博主..."
    - name: "测评"
      prompt: "你是一个产品测评师..."

# 数据库配置
database:
  path: "./data/xhs_toolkit.db"
  backup_interval: 86400  # 每天备份
```

---

## 8. 依赖项

### 8.1 Python 依赖

```txt
# requirements.txt
playwright>=1.40.0
requests>=2.31.0
aiohttp>=3.9.0
sqlalchemy>=2.0.0
pydantic>=2.5.0
python-dotenv>=1.0.0
apscheduler>=3.10.0
pillow>=10.0.0
beautifulsoup4>=4.12.0
lxml>=4.9.0
```

### 8.2 系统依赖

```bash
# 安装 Playwright 浏览器
playwright install chromium

# 安装图像处理库
# (Pillow 通常自带)
```

---

## 9. API Keys 汇总

| 服务 | 用途 | Key 位置 |
|------|------|----------|
| **Grsai** | AI 写作 | config.yaml |
| **BoCha** | 热点搜索 | config.yaml |
| **小红书** | 爬虫/发布 | 环境变量/配置文件 |

---

## 10. 开发优先级

### Phase 1: 核心功能 (必须)

1. **热点追踪爬虫** - `xhs-hot-tracker`
   - 热门笔记爬取
   - 热门话题获取
   
2. **AI 写作** - `xhs-ai-writer`
   - 标题生成
   - 文案生成
   - 标签生成
   
3. **自动发布** - `xhs-publisher`
   - 登录
   - 发布笔记
   - 图片上传

### Phase 2: 增强功能

4. **数据追踪** - `xhs-data-tracker`
5. **定时发布** - `xhs-scheduler`
6. **图片处理** - `xhs-image-generator`

### Phase 3: 高级功能

7. **爆款分析** - `xhs-virus-analyzer`
8. **数据报表** - `xhs-analytics`
9. **素材搜索** - `xhs-content-finder`

---

## 11. 文件结构

```
Package_1/
├── skills/
│   ├── xhs-hot-tracker/        # 热点追踪
│   │   ├── SKILL.md
│   │   └── scripts/
│   │       ├── main.sh
│   │       └── crawler.py
│   │
│   ├── xhs-ai-writer/          # AI 写作
│   │   ├── SKILL.md
│   │   └── scripts/
│   │       ├── main.sh
│   │       └── writer.py
│   │
│   ├── xhs-publisher/          # 自动发布
│   │   ├── SKILL.md
│   │   └── scripts/
│   │       ├── main.sh
│   │       └── publisher.py
│   │
│   ├── xhs-data-tracker/      # 数据追踪
│   │   ├── SKILL.md
│   │   └── scripts/
│   │       ├── main.sh
│   │       └── tracker.py
│   │
│   ├── xhs-image-generator/    # 图片处理
│   │   ├── SKILL.md
│   │   └── scripts/
│   │       └── image_tool.py
│   │
│   └── xhs-scheduler/          # 定时任务
│       ├── SKILL.md
│       └── scripts/
│           └── scheduler.py
│
├── tools/
│   └── (现有工具保留)
│
├── config/
│   └── config.yaml             # 统一配置
│
├── data/
│   └── xhs_toolkit.db         # SQLite 数据库
│
└── docs/
    └── (现有文档保留)
```

---

## 12. 测试用例

### 12.1 热点追踪测试

```bash
# 测试获取热门笔记
xhs-hot get notes --category 美食 --limit 10

# 测试获取热门话题
xhs-hot get topics

# 预期输出：JSON 格式的笔记列表
```

### 12.2 AI 写作测试

```bash
# 测试生成标题
xhs-ai title --topic "面膜推荐" --style 种草

# 测试生成文案
xhs-ai content --topic "面膜推荐" --length medium

# 预期输出：生成的文案
```

### 12.3 发布测试

```bash
# 测试登录
xhs-publisher login

# 测试发布
xhs-publisher publish --title "测试" --content "测试内容" --images /path/to/image.jpg

# 预期输出：发布成功的笔记 URL
```

---

## 13. 注意事项

1. **Cookies 管理**
   - 小红书 cookies 需要定期刷新
   - 建议每次使用前验证有效性

2. **频率限制**
   - 爬取需要控制频率，避免被封
   - 建议间隔 3-5 秒

3. **反爬策略**
   - 需要模拟正常用户行为
   - 可能需要代理 IP

4. **内容合规**
   - 生成的文案需符合小红书社区规范
   - 避免敏感词、违规词

---

**文档版本**: 1.0  
**创建日期**: 2026-02-13  
**维护者**: Wilson
