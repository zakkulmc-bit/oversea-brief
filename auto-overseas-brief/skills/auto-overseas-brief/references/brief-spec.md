# Brief Specification

## Output

- Detail file: `briefs/YYYY-MM-DD.html`
- Archive file: `index.html`
- Data config file: `brief-data/YYYY-MM-DD.json`
- Share image file: `share-images/YYYY-MM-DD.png`
- Detail structure: opening line, 4 sections x 3 items, closing insight, sourcing note, footer
- Archive structure: latest card first; every existing brief remains clickable

## Sections

汽车行业出海简报的固定栏目（来源优先级与采集关键词见 `sources.md`）：

| Section | Focus | Seed queries |
| --- | --- | --- |
| 出口与海外数据 | 出口规模与结构、海外销量与市场份额 | 中国汽车出口, CAAM 出口数据, 海关总署, NEV export, SMMT, ACEA |
| 政策法规 | 国内外准入、关税、贸易与产业政策 | 商务部, 工信部, 欧盟关税, 价格承诺, 反补贴, 进口法规 |
| 行业研究 | 智库报告、出海战略、投行研判 | 中汽政研, 车百智库, 亿欧智库, 国际化联盟, 投行研报 |
| 行业动态 | 车企出海事件、本地化、渠道与产品 | 比亚迪, 奇瑞, 上汽, 海外建厂, 经销渠道, 盖世汽车 |

每个栏目保持 3 条；如需改造为其他行业，保持 4 栏目 x 3 条结构，并先替换栏目名称、采集关键词、来源优先级与判断维度。

## Archive Layout

- Archive/homepage uses a neutral black/white/gray shell even when the detail pages use weekday Morandi colors.
- Top visible brand text is Chinese-first: `每日汽车出海简报`; upper right remains `星期一研究室`.
- Top navigation labels are Chinese and ordered: `今日`, `色板`, `往期`.
- The hero section keeps the English publication masthead `The Auto Overseas Brief`, a phone-shaped daily-paper preview, and three action buttons:
  - `阅读最新一期` links to the latest detail page.
  - `查看七天色板` switches to the palette panel without forcing scroll-to-top.
  - `查看往期` switches to the previous-issues panel without forcing scroll-to-top.
- The archive/recommendation panel is visibly named `往期`; do not use `历史归档` or `精彩推荐` as the section heading.
- Weekly palette on the homepage uses individual swatch archive cards with weekday shorthand, Chinese tone name, and hex code. Avoid plain oval/pill strips.
- If homepage card markup changes, update the Feishu push worker archive parser so scheduled pushes can still locate the latest issue headline and summary.

## Detail Layout

- Detail pages use a two-column editorial layout.
- Detail pages should feel like a landscape A3 editorial sheet, not a tall poster:
  - use a wide page frame around `1480px`
  - use the A3 landscape ratio as a minimum paper-height reference, not as a fixed crop box
  - allow vertical overflow to scroll naturally; never crop lower sections, insight, or footer
  - use generous left/right page margins and clear row spacing between section pairs
  - keep generous vertical breathing room around the opening line and closing insight so major editorial blocks do not touch
  - style the opening line as an international editorial pull quote: thin top/bottom rules, a non-numeric `LEAD` rail, strong serif text, and a right-side illustration panel; avoid rounded AI-card styling.
  - style the closing insight as an editorial note: top/bottom rules, a left label with a small theme-colored signal icon, and larger serif body text; avoid office-report card styling.
- The visual language should lean toward an international editorial atlas:
  - use a faint paper grid inside the page frame
  - place international editorial illustration inside the opening-line card as a right-side visual panel on desktop, not between section cards or near the footer
  - prefer original inline SVG figures: globe grids, route arrows, car silhouettes, source-ledger tags, export-atlas marks, or archive labels
  - avoid barely visible route paths and repeated paper-plane marks
  - use English micro-labels that fit the publication theme, such as `AUTO INDUSTRY MAP`, `GLOBAL SIGNALS`, `SOURCE LEDGER`, and `EXPORT ATLAS`; avoid vague placeholder phrases.
  - keep section cards clean; decorative illustration should not cross over article text
  - hide heavy illustration ornaments on mobile
- Section order is horizontal:
  - top row: `01 出口与海外数据`, `02 政策法规`
  - bottom row: `03 行业研究`, `04 行业动态`
- Each section header uses a skewed numbered marker and a subtle dashed guide line.
- Item markers inside each section use non-numeric symbols (`◆`, `◇`, `◈`) so they do not compete with section numbers.
- The section marker color must be derived from the day's `--primary` Morandi color:
  - `--section-ink: var(--primary)`
  - `--section-ink-soft: var(--primary-light)`
  - `--section-guide: color-mix(in srgb, var(--primary) 42%, transparent)`
- The top masthead rule should also derive from the day's `--primary`, using a slightly darker mix for newspaper weight.
- Do not hard-code an unrelated blue or accent color for section markers.
- Item rows should dynamically align left/right corresponding items by measured content height on desktop; mobile stays natural single-column.

## Editorial Rules

- Prefer sources published in the seven days up to the brief date.
- Use official/primary URLs wherever available (监管机构、行业协会统计、智库研报、权威行业媒体).
- Add visible dates to source links. Mark items outside the preferred window as `邻近窗口` or `最近官方参考`.
- Daily draft data must be prepared in `brief-data/YYYY-MM-DD.json` before HTML generation.
- New daily items must not duplicate historical entries by source URL or item title.
- Title form: `品牌/机构/产品名 | 核心动作短语`; action phrase no longer than 15 Chinese characters.
- Description form: fact (数据/政策/研报结论/事件), then meaning for 出海运营; target 60-80 Chinese characters.
- Opening line: one judgment about 汽车出海 structural shift, maximum 50 Chinese characters.
- Closing insight: synthesize multiple sections, maximum 150 Chinese characters; avoid tentative filler.

## Brand

- Public name: `The Auto Overseas Brief`
- Chinese title: `汽车出海全球日报`
- Homepage brand text: `每日汽车出海简报`
- Producer attribution in upper right: `星期一研究室`
- Detail footer left: `星期一研究室出品`
- Detail footer right: `汽车出海全球日报 · 行业动态 · 机会判断 · 来源可追溯`
- Archive title: `The Auto Overseas Brief`

For a new industry, replace the public name and footer terms while preserving the template hierarchy.

## Palette

| Day | Primary | Tone |
| --- | --- | --- |
| 星期一 | `#927BBE` | 莫兰迪紫 |
| 星期二 | `#6F97A8` | 雾蓝 |
| 星期三 | `#7FA6C9` | 晴蓝 |
| 星期四 | `#7FA68B` | 鼠尾草绿 |
| 星期五 | `#6F9F99` | 青瓷绿 |
| 星期六 | `#8A93B7` | 紫蓝 |
| 星期日 | `#EC9BC8` | 柔粉 |

Use an approximately 10%-tinted pale background derived from the day's primary color for callouts and counters.
Use the same day's primary color family for section number markers, marker shadows, and guide lines.
Keep the weekly palette balanced around purple, blue, green, and pink families; do not use brown, orange, or honey-gold weekday accents.

## Existing Site

- Repository: `zakkulmc-bit/oversea-brief`
- Site directory: `auto-overseas-brief/`
- Publish from `main` through GitHub Pages.

## Release Gate

1. No template placeholders remain.
2. The new page contains exactly 4 section containers, 12 content items, and 12 source anchors.
3. Dedup check passes: `node skills/auto-overseas-brief/scripts/check-brief-dedup.js brief-data/YYYY-MM-DD.json`.
4. The archive contains a clickable card for the new page and preserves earlier links.
5. Browser inspection shows clean header/footer, no obvious clipping, and functioning navigation.
6. The share image `share-images/YYYY-MM-DD.png` exists, is a PNG screenshot of the detail page, and is published with the same issue.
7. If the site is published, verify the deployed archive, current detail page, and current share image after Pages builds. Deployment is complete only when the archive contains `briefs/YYYY-MM-DD.html`, the detail page shows the correct date/footer, and `share-images/YYYY-MM-DD.png` returns `image/png`.
8. After Pages deployment verification succeeds, immediately trigger the Feishu image push with `npm --prefix workers/feishu-brief-push run post-publish-send -- --date YYYY-MM-DD`, or call `/send-image-url` with the published PNG when the Worker screenshot path is unavailable. Do not rely only on the scheduled patrol. Manual trigger commands require `MANUAL_TRIGGER_TOKEN` or `FEISHU_PUSH_TOKEN`.
