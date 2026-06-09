# The Auto Overseas Brief — Skill

每日「汽车行业出海简报」工作流，由 [`ai-industry-brief`](https://github.com/mondaylab/ai-industry-brief) 扩展而来。面向**汽车行业出海运营团队**，每天生成一份简洁、来源可追溯的出海简报，并维护可发布的静态 HTML 归档站点。

保留原 Skill 的数据结构、去重脚本、HTML 模板、发布流程与质量检查；替换了栏目、采集关键词、来源优先级、写作判断维度、品牌文案和示例 `brief-data` 模板。

## 栏目

| # | 栏目 | 一级来源 | 二级来源 |
| --- | --- | --- | --- |
| 01 | 出口与海外数据 | CAAM、海关总署商务数据中心、中汽协统计信息网 | Global NEVS、SMMT、ACEA、盖世出口数据库 |
| 02 | 政策法规 | 商务部官网、工信部官网 | 商务部政策法规库、地方商务厅网站 |
| 03 | 行业研究 | 中汽政研、汽车企业国际化发展创新联盟、车百智库 | 亿欧智库、投行研报 |
| 04 | 行业动态 | 盖世汽车、每日经济新闻、中国汽车报 | 中国汽车要闻、China Auto & Parts |

## 使用

```bash
# 安装（如发布为独立 Skill 仓库）
npx skills add zakkulmc-bit/auto-overseas-brief -y -g
```

在配置好的站点目录（含 `brief-data/`、`briefs/`、`index.html`）中：

```bash
# 1. 复制模板并填写当天数据
cp skills/auto-overseas-brief/brief-data/_template.json brief-data/YYYY-MM-DD.json

# 2. 去重检查（写 HTML 前必须通过）
node skills/auto-overseas-brief/scripts/check-brief-dedup.js brief-data/YYYY-MM-DD.json

# 3. 渲染详情页 + 更新归档首页
node skills/auto-overseas-brief/scripts/render-brief.js YYYY-MM-DD
```

## 文件

- `SKILL.md` — Skill 主入口、触发话术、调研/写作/构建/发布流程。
- `references/brief-spec.md` — 内容规则、版式、色板、品牌、发布检查门槛。
- `references/sources.md` — 栏目、采集关键词、来源优先级、写作判断维度。
- `assets/brief-page-base.html` — 详情页视觉基础模板（横版 A3 编辑版式）。
- `assets/archive-page-base.html` — 归档首页基础模板（黑白灰外壳 + 七天色板）。
- `scripts/check-brief-dedup.js` — 4 栏目 × 3 条结构校验 + 历史去重（来源 URL/标题）。
- `scripts/render-brief.js` — 由 `brief-data` JSON 渲染详情页并更新首页。
- `brief-data/_template.json` — 每日数据源示例模板。

## 品牌

- 公开名：`The Auto Overseas Brief`
- 中文名：`汽车出海全球日报`
- 出品方：`星期一研究室`
