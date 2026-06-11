---
name: auto-overseas-brief
description: 创建、初始化并维护可追溯来源的每日「汽车行业出海简报」和静态 HTML 归档。当用户要求安装或使用 auto-overseas-brief 工作流，初始化汽车出海行业简报，调研中国汽车出口、海外市场、政策法规、行业研究与行业动态的每日动态，生成 brief-data JSON，渲染简报页面，更新归档首页，通过 GitHub Pages 发布，或把这套流程复用到自动化、CLI、Skill 中时使用。面向汽车行业出海运营团队。
---

# The Auto Overseas Brief

为汽车行业出海运营团队生成一份简洁、有来源依据的每日出海简报，并维护可发布的 HTML 归档站点。本 Skill 由 `ai-industry-brief` 扩展而来，保留数据结构、去重脚本、HTML 模板、发布流程与质量检查，仅替换栏目、采集关键词、来源优先级、写作判断维度、品牌文案和示例模板。

## 安装触发

当用户要求从 GitHub 安装这个 Skill 时，优先使用标准 Skills 安装器：

```bash
npx skills add zakkulmc-bit/auto-overseas-brief -y -g
```

安装后，如果宿主环境需要重新发现 Skill，提醒用户重启或开启新会话。随后继续完成用户要求的初始化或第一期简报生成。

以下用户话术都应该触发本 Skill：

- `请帮我安装 auto-overseas-brief 这个汽车出海简报 Skill，并初始化一个项目。`
- `请用 auto-overseas-brief Skill 生成今天的汽车行业出海简报。`
- `基于这个 Skill 搭一个汽车出海日报，不要让我手动配。`

## 开始

1. 读取 [references/brief-spec.md](references/brief-spec.md)。
2. 读取 [references/sources.md](references/sources.md)，获取栏目、采集关键词、来源优先级和写作判断维度。
3. 如果是在更新已有站点，先读取最新的 `briefs/YYYY-MM-DD.html` 和 `index.html`，保留已有布局和历史归档。
4. 如果是在新建站点，使用 `assets/` 里的 HTML 基础模板，并替换所有示例内容、日期、颜色和链接。
5. 创建或更新 `brief-data/YYYY-MM-DD.json`。这个 JSON 是当天简报的唯一数据源，通常从 `brief-data/_template.json` 复制得到。

## 零配置初始化

当用户想做汽车出海简报，但还没有配置项目时，替用户完成初始化，不要把配置清单丢回给用户。

1. 默认行业为「汽车行业出海」，目标读者为「汽车行业出海运营团队」。
2. 直接采用本 Skill 预设的 4 个栏目、采集关键词、来源优先级和判断维度，见 [references/sources.md](references/sources.md)。
3. 如果项目结构不存在，创建 `brief-data/`、`briefs/`、`skills/auto-overseas-brief/` 和 `index.html`。
4. 将 `brief-data/_template.json` 复制为 `brief-data/YYYY-MM-DD.json`，并替换栏目内容、首页手机预览栏目和示例副标题。
5. 先生成第一期简报，再让用户调整高级设置。
6. 只有在第一期已经生成之后，才继续询问是否接入 GitHub Pages 发布、飞书群推送或定时任务。

目标体验是“一句话到第一期”。除非外部账号、权限或部署选择阻塞流程，否则不要让用户自己照着清单操作。

## 调研

1. 使用当前网络搜索，优先选择官方发布、监管机构、行业协会统计、智库研报或权威行业媒体等一手来源。
2. 围绕 4 个已配置栏目检索：`出口与海外数据`、`政策法规`、`行业研究`、`行业动态`。各栏目的来源优先级见 [references/sources.md](references/sources.md)。
3. 每个栏目选择 3 条值得写入的动态，优先最近 7 天。
4. 每条动态必须记录真实 URL 和发布日期。窗口外资料要标注 `邻近窗口` 或 `最近官方参考`，不要暗示它是今天的新消息。
5. 写 HTML 前先运行去重检查，并解决所有冲突：
   - `node skills/auto-overseas-brief/scripts/check-brief-dedup.js brief-data/YYYY-MM-DD.json`

### 搜索后端

内置 `WebSearch` 在中国大陆区域不可用（US-only，返回训练数据占位文本而非实时结果）。当搜索不可用时，按以下优先级切换：

1. **如果已配置博查 Bocha MCP**（推荐，国内直连免代理）——使用 `mcp__bocha__bocha_web_search`，每次搜索带 `freshness: "oneWeek"` 优先取最近 7 天结果
2. **如果有代理**（HTTP/SOCKS 可达）——给 git 和 curl 配上代理，可用 Tavily MCP 或其他境外搜索 API
3. 博查 MCP 注意：
   - 正确包名是 `@iflow-mcp/yoko19191-bocha-ai-mcp-server`（不要用 `@humansean/mcp-bocha`，那个包已损坏，启动即崩）
   - `freshness` 过滤不严格，会混进旧数据；必须**按结果里的真实发布日期**（`2026-0x-xx`）筛选，不要依赖 freshness 参数
   - Bash 下发 curl 测试 API 时，JSON body 用单引号在 Git Bash 下会报 "Missing request body"；改用 `--data @临时文件` 传参

## 写作

1. 写一句明确判断式开场，控制在 50 个中文字符以内，落在汽车出海的结构性变化上。
2. 12 条动态都使用 `品牌/机构/产品名 | 核心动作短语` 格式，动作短语控制在 15 个中文字符以内。
3. 每条描述先写事实（数据、政策、研报结论或事件），再写对出海运营的影响，长度约 60-80 个中文字符。
4. 写一段跨栏目洞察，控制在 150 个中文字符以内，必须有明确判断，避免模糊套话。
5. 判断维度参考 [references/sources.md](references/sources.md)：市场准入、关税与合规成本、产能与本地化、渠道与品牌、出口规模与结构。

## 构建

1. 根据 `brief-data/YYYY-MM-DD.json` 生成 `briefs/YYYY-MM-DD.html`，使用规格文档中的星期色板和品牌规则。
2. 更新 `index.html`，包含最新一期、`往期`推荐列表和七天色板入口；保留历史简报链接。

### 首期 Bootstrap

`render-brief.js` 用最新已有 `briefs/*.html` 当模板生成新详情页。第一期时 `briefs/` 为空 → 脚本报 `No existing brief template found`：

- **第一期**：直接复制 `assets/brief-page-base.html`，手动替换所有内容（日期、开场、四栏目 12 条、洞察、页脚、主题色），另存为 `briefs/YYYY-MM-DD.html`
- **首页**：复制 `assets/archive-page-base.html` 为 `index.html`，替换手机预览和卡片内容
- **第二期起**：运行 `node skills/auto-overseas-brief/scripts/render-brief.js YYYY-MM-DD`，脚本会自动用上一期当模板

`render-brief.js` 更新 index 的正则对空白敏感；匹配不上时报 `Unable to replace index brief grid`。如果发生，手动编辑 index.html：
- 把 `brief-grid` 里的 featured card 替换为最新期（日期、标题、摘要、链接）
- 把原 featured 降为"查看前一期 →"普通卡片
- 在 `archive-list` 顶部插入最新期的 `archive-item`
- 更新 `阅读最新一期` 按钮和手机预览区

### 生成分享图

3. 每期在渲染后立即生成分享图 `share-images/YYYY-MM-DD.png`，作为飞书图片卡的稳定兜底素材。生成方式：

```bash
# headless Edge（Windows 默认可用，免装依赖）
EDGE="/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe"
"$EDGE" --headless=new --disable-gpu --hide-scrollbars \
  --force-device-scale-factor=2 \
  --window-size=1560,1640 \
  --screenshot="<输出PNG绝对路径>" \
  "file:///<详情页绝对路径HTML>"
```

- `--window-size` 高度至少 1640，否则会截掉 03/04 栏目和洞察/页脚
- 输出需返回 `PNG image data` 才算成功；不依赖 Playwright / Puppeteer
- 生成完后用 `file` 命令验证：`share-images/YYYY-MM-DD.png: PNG image data, ...`

4. 每期简报都必须保留可点击来源链接和来源日期标注。
5. 保留基础模板里的归档页/首页布局：
   - 首页外层保持黑、白、灰的中性色，不要用当天主题色污染全局页面框架。
   - 顶部导航使用中文标签，顺序为：`今日`、`色板`、`往期`。
   - Hero 按钮使用：`阅读最新一期`、`查看七天色板`、`查看往期`；切换 Tab 的按钮不要强制页面滚回顶部。
   - 使用手机形状预览来表达“一个手机里装着一份每日小报”。
   - 首页七天色板使用色卡归档卡片，包含 `MON`-`SUN`、中文色调名和 HEX 色值，不使用普通胶囊条。
   - 可见的归档/推荐区名称使用 `往期`，不要写成 `历史归档` 或 `精彩推荐`。
6. 保留基础模板里的详情页双栏编辑版式：
   - 桌面端页面更接近横版 A3 编辑页，不要做成长图海报。
   - 汽车出海简报的栏目顺序是：上排 `01 出口与海外数据`、`02 政策法规`；下排 `03 行业研究`、`04 行业动态`。
   - 保留低对比 route-map 图层、纸张网格和 waypoint 风格栏目卡片，形成国际化编辑地图报刊感。
   - 栏目数字标记、阴影和引导线必须来自当天的 `--primary` 和 `--primary-light` 色彩。
   - 栏目内部条目标记使用非数字符号（`◆`、`◇`、`◈`），不要使用 `01/02/03`。
   - 生成或编辑详情页时，保留桌面端动态行高对齐脚本。

## 检查与发布

1. 检查是否还有未替换占位符，并确认包含 4 个栏目、12 条内容和 12 个来源链接。
2. 发布前重新运行去重检查，确保与历史简报没有重复。
3. 在浏览器打开归档页和新详情页，确认卡片导航、头部、页脚、换行、小屏可读性、栏目横向顺序和莫兰迪主题色标记都正常。
4. 如果用户要求发布，或仓库已经配置发布流程，只提交相关站点和 Skill 文件，推送到配置好的 GitHub 仓库，并在部署后验证 GitHub Pages URL。
5. GitHub Pages 当前部署完全生效后，立即触发飞书图片推送，不要只等待 cron 巡逻。部署完成门槛是：归档首页已包含当天详情页链接、详情页日期/页脚正确、`share-images/YYYY-MM-DD.png` 已在公开站点返回 PNG：
   - `MANUAL_TRIGGER_TOKEN=<token> npm --prefix workers/feishu-brief-push run post-publish-send -- --date YYYY-MM-DD`
   - 该脚本会等待上述部署门槛通过后再触发 Worker，避免 Pages 仍为旧版本或 PNG 尚未生效时发送链接卡。
   - 如果本地环境没有 `MANUAL_TRIGGER_TOKEN` 或 `FEISHU_PUSH_TOKEN`，但当前 Wrangler 已登录且能管理 Worker secrets，可以临时轮换 `MANUAL_TRIGGER_TOKEN`，用新 token 调用 `/send-image-url?date=YYYY-MM-DD&image_url=<published_png_url>`，成功后删除本地临时 token 文件，不要在输出或记忆中记录 token 明文。
   - 如果无法取得或轮换触发 token，明确报告“站点已发布，但发布后飞书即时推送缺少 Worker 手动触发 token”，并说明定时 Worker 仍会按 cron 巡逻；此时因为 `share-images/YYYY-MM-DD.png` 已发布，Worker 截图失败时也应优先发送该图片而不是链接卡。
6. 如果这个流程由定时任务驱动，保持品牌、页脚、归档页、Worker 解析逻辑和发布说明与本 Skill 同步。

## 资源

- 读取 [references/brief-spec.md](references/brief-spec.md)，获取内容规则、颜色、品牌、文件路径和发布检查要求。
- 读取 [references/sources.md](references/sources.md)，获取汽车出海的栏目、采集关键词、来源优先级和判断维度。
- 使用 [assets/brief-page-base.html](assets/brief-page-base.html) 作为新详情页的视觉基础模板。
- 使用 [assets/archive-page-base.html](assets/archive-page-base.html) 作为新归档页的视觉基础模板。
- 使用 `brief-data/_template.json` 创建每日配置，用 `scripts/check-brief-dedup.js` 防止重复。
