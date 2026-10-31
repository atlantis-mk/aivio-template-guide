# Aivio 模板开发指南

面向**模板作者**的简短规范：怎样建一个会被 **Aivio** 桌面应用的模板中心发现并正确展示的 GitHub 公开模板仓。

> **本仓是文档仓，不是视频模板。** 请勿给本仓挂 topic `aivio-template`，否则会误出现在模板中心。

本指南只约定作者怎么写仓库。**不改** Aivio 产品解析／卡片代码。只有卡片的模板写 `aivio.*` 即可；Remotion 一类工程模板在同一份根 `README.md` 里同时保留制作用工程字段，并另写 `aivio.*`（见第 4 节）。

---

## 1. 怎样被发现

同时满足以下条件，仓库才会出现在模板中心列表：

1. 仓库可见性为 **Public**
2. Repository topics 含精确字符串 **`aivio-template`**（与产品常量一致）
3. 默认分支根目录有 **`README.md`**（优先此大小写）

**缺元数据不会被踢出列表。** 漏写 title／description／cover 时，卡片仍会出现，但会显示「未命名模板」／「无简介」／统一封面占位。规范上作者**应当**补齐 `aivio.title` 与 `aivio.cover`。

---

## 2. README 元数据（卡片真源）

在默认分支根目录 `README.md` **文件开头**写 YAML frontmatter（首行 `---`，下一独立行 `---` 结束），使用命名空间 **`aivio`**：

```markdown
---
aivio:
  title: "三维 Remotion 口播模板"
  description: "适合口播 + 三维片头的一期成片起步"
  cover: "cover.png"
---

# 正文标题（任意；不作为卡片标题）
```

| 键 | 作者规范 | 卡片效果 |
| --- | --- | --- |
| `aivio.title` | **应写**非空字符串；用人话说明模板类型／用途 | 卡片**标题**；缺／空 →「未命名模板」 |
| `aivio.description` | **可选** | 有则展示；无 →「无简介」 |
| `aivio.cover` | **应写**；相对仓库根的路径；**推荐 `cover.png`** | 有且可拉取 → 封面图；否则 CSS 占位 |

### 常见误解（请勿再这么做）

| 错误做法 | 正确做法 |
| --- | --- |
| 用仓库名／`owner/repo` 当卡片标题 | 标题只来自 `aivio.title`；`owner/repo` 是**副标题** |
| 用 GitHub About 的 Description 当卡片描述 | 描述只来自 `aivio.description` |
| 只写工程 `name`／顶层 `description` | 卡片不读它们；须另写 `aivio.title`／`aivio.description` |
| 只靠 Open Graph／社交预览图当封面 | 封面真源是 `aivio.cover` 指向的仓内文件 |
| 用工程 `reference_files.cover` 当卡片封面 | 那是制作参考图；卡片封面写 `aivio.cover` |
| 默认把封面放在 `aivio/cover.png` | **推荐改为根目录 `cover.png`**（见下一节） |

产品解析接受**任意合法相对路径**（禁止 `http(s):`、禁止以 `/` 开头、禁止路径中的 `..` 段）。因此把推荐封面改到根目录 **不需要**改 Aivio 解析代码；只要 `aivio.cover` 写成实际路径即可。

---

## 3. 封面规范

| 项 | 约定 |
| --- | --- |
| **推荐路径** | 仓库**根目录** **`cover.png`** |
| 亦允许 | 根目录 `cover.jpg`／`cover.webp`，或其它合法相对路径；`aivio.cover` 必须写成**实际文件路径** |
| 与旧推荐 | 产品侧早期文档曾推荐 `aivio/cover.png`。**作者规范以本指南为准**：新模板请用根目录 `cover.png`。若你仍使用 `aivio/cover.png` 且 frontmatter 写对、文件存在，产品仍可展示（合法相对路径） |
| 路径规则 | 相对仓库根；禁止 `http(s):`、禁止以 `/` 开头、禁止 `..` 段 |
| 建议尺寸 | **16:9**；长边建议 ≥ **1280px**；文件宜小（建议 < 500KB）以便列表加载 |
| 缺失时 | 软件显示统一 CSS 占位；**仍可被发现** |

---

## 4. 工程元数据 + aivio 展示元数据

有些模板的根 `README.md` 还带一套**工程字段**，给制作流程、Skill 和流水线用。样板是 [科技透视动效视频模板](https://github.com/atlantis-mk/kinetic-tech-remotion-template)（kinetic-tech V2）。工程字段与 `aivio` **写在同一段 frontmatter**，两套键各留各的：加卡片时保留工程键，写工程字段时也补上 `aivio`。

**模板中心卡片只认 `aivio.title`／`aivio.description`／`aivio.cover`。** 缺了仍按第 2 节回退（「未命名模板」／「无简介」／CSS 占位），列表不过滤。产品已支持这三键；双轨是作者写法，不需要改 Aivio。

科技模板请保留现有工程键，至少包括：`schema_version`、`id`、`name`、`version`、`description`、`status`、`engine`、`scope`、`tags`、`entrypoints`、`paths`、`accepted_inputs`、`video_aspect_ratios`、`default_deliverables`、`capabilities`、`reference_files`。其它仓以自己 README 里已有的键为准，勿删。

### 版本号各写各的

| 字段 | 写在哪 | 怎么写 | 表示什么 |
| --- | --- | --- | --- |
| `schema_version` | README **工程** frontmatter | 科技模板 V2 为 **`"2.0"`**（与组件目录 `component-catalog.json` 同一批） | 工程元数据这一代。**与卡片协议无关**，也与 `aivio.*` 无关 |
| `schemaVersion` | `content.json` | **保持 `"1.0"`** | 片子仍是 `scenes[].kind` + `data` |
| （无版本键） | `aivio` 块 | 只写 title／description／cover | 卡片展示 |

README 升到 `"2.0"` 时，不要把 `content.json` 的 `schemaVersion` 一起改掉。尚未跟进 V2 的其它 `aivio-template` 仓可以继续用工程 `"1.0"`，只要补齐 `aivio.*`；卡片规则相同。

### 封面仍推荐根目录 `cover.png`

`aivio.cover` 写 `"cover.png"`，并在仓库根放同名文件（建议 16:9，长边 ≥ 1280）。`reference_files.cover` 继续指向片内参考图，不代替卡片封面。

### `layers`／`transition`（L0）

V2 **还不要求**模板作者写 `scenes[].layers` 或跨场景 `transition`。出片继续用 `kind` + `data`。这两项只是后续方向：本轮不执行，也不要写进当前 `content.json`。

### 可复制片段（对齐 kinetic-tech V2）

片段已截断。真实仓库保留全部工程键（含 `capabilities` 等），追加 `aivio`，不要用这段覆盖整份 README。

```yaml
---
schema_version: "2.0"          # 工程字段；与卡片协议无关，也与 aivio.* 无关
id: kinetic-tech-remotion-template
name: "科技透视动效视频模板"
engine: remotion
entrypoints:
  instructions: "AGENTS.md"
  skill: ".agents/skills/make-video/SKILL.md"
  template: ".agents/skills/make-video/references/template.md"
paths:
  reference: ".agents/skills/make-video/assets/reference/"
  source: ".agents/skills/make-video/assets/project-template/"
  workspace: "work/"
  input: "work/input/"
  delivery: "work/delivery/"
# … version、description、status、capabilities、reference_files 等其余工程键全部保留 …
aivio:
  title: "科技透视动效视频模板"
  description: "用芯片透视、数据墙、结构拆解与空间卡片制作科技解说视频，支持参数化镜头、程序音效和三种封面。"
  cover: "cover.png"
---
```

`aivio.title` 建议与工程 `name` 的人话一致，可以微调。`aivio.description` 可与顶层 `description` 相同或更短。`speech_semantic_alignment` 与 `automatic_voiceover_generation` 若仍未实现，`capabilities` 保持 `not_implemented`。

---

## 5. 完整示例（可复制）

下面是一份可直接粘贴到**只有卡片、没有工程字段**的新模板仓根 `README.md` 的全文。带工程字段的 Remotion 模板用第 4 节片段，并保留原有工程键。请按需改 title／description，并在仓库根放入对应的 `cover.png`。

````markdown
---
aivio:
  title: "三维 Remotion 口播模板"
  description: "适合口播 + 三维片头的一期成片起步"
  cover: "cover.png"
---

# 三维 Remotion 口播模板

一句话介绍：用 Remotion 做一期带三维片头的口播成片起步模板。

## 适用场景

- 口播 + 三维开场的短视频／栏目片头
- 需要固定版式、快速替换旁白与素材的创作者

## 使用前材料清单

- 口播稿或提词文本
- 主视觉／LOGO（可选）
- 背景音乐或音效（可选，注意版权）

## 目录结构

```text
.
├── README.md      # 含本文件开头的 aivio frontmatter
├── cover.png      # 卡片封面（与 aivio.cover 一致）
└── …              # 你的模板工程文件
```

## 许可／署名

（可选）说明许可证与署名要求。
````

建好公开仓、挂上 topic `aivio-template`、放入 `cover.png` 后，在 Aivio 模板中心刷新，应看到：

- **标题**：frontmatter 里的人话标题  
- **副标题**：`owner/repo`  
- **描述**：有则展示，无则「无简介」  
- **封面**：`cover.png`（或占位）

---

## 6. 作者自检清单

发布或更新模板前请勾一遍：

- [ ] 仓库为 **Public**
- [ ] Topics 含精确 **`aivio-template`**
- [ ] 根目录 `README.md` 含合法 `aivio:` frontmatter（文件开头的 `---` … `---`）
- [ ] `aivio.title` 非空，且不是拿仓库名或工程 `name` 凑数
- [ ] （可选）写了 `aivio.description`
- [ ] 根目录存在封面文件，且 `aivio.cover` 指向它（推荐 **`cover.png`**）
- [ ] 若 README 还有工程字段：`id`／`name`／`engine`／`entrypoints`／`paths`／`schema_version` 等仍在，并另有 `aivio.*`
- [ ] 工程 `schema_version`（科技模板 V2 为 `"2.0"`）与 `content.json` 的 `schemaVersion`（保持 `"1.0"`，`kind` + `data`）分开写；前者与卡片协议无关
- [ ] 未把 `layers`／`transition` 写成当前必填
- [ ] 在 Aivio 模板中心刷新后：标题／副标题（`owner/repo`）／描述／封面符合预期

---

## 7. 本指南明确不写的内容

- 真费、计费、付费模板市场  
- zip 下载实现细节、T3 Agent 协议、「使用模板」弹窗字段顺序  
- 教人改 Aivio 源码或私有 API（产品已读 `aivio.*`；缺省回退保持现状）  
- 把「缺元数据」写成「会被列表过滤」（不会）  
- 把 `layers`／`transition` 写成 V2 作者必做（L0，后续才做）  
- 通用 JSON 视频 DSL、跨模板的全局 Skill，或把组件化写成已有自动配音／语义对齐

---

## 8. 对照与维护

| 项 | 说明 |
| --- | --- |
| 产品发现 topic | `aivio-template` |
| 卡片标题／描述／封面真源 | README `aivio.*` frontmatter（线 M 已合入） |
| 双轨 README | 工程字段与 `aivio.*` 同文件；卡片只认 `aivio.*` |
| 工程 `schema_version` | 科技模板 V2 为 `"2.0"`；**与卡片协议无关**；与 `content.json` 的 `schemaVersion: "1.0"` 无关 |
| 作者推荐封面 | **根目录 `cover.png`**（本指南；取代旧文案中的 `aivio/cover.png`） |
| `layers`／`transition` | L0：V2 作者暂不编写 |
| 工程样板 | [kinetic-tech-remotion-template](https://github.com/atlantis-mk/kinetic-tech-remotion-template) |
| 本仓 | 文档仓；**不要**挂 `aivio-template` |

若日后再次调整作者推荐路径，请先改本指南；仅当产品路径**合法性规则**变更时才需要改 Aivio 代码。
