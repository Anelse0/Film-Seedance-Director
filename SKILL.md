---
name: film-seedance-director
description: 影视创作到 Seedance 2.5 Prompt 的工作流：概念、故事、剧本、表演、分镜、参考资产与质量检查。支持调用情绪表演原文、调节强度与克制、微调和重组情绪弧线及独立定时表演测试。用于短片创作、剧本转视频 Prompt、视频编辑/延长规划和成片修正；不用于单张图片生成或非视频文案。
---

# Film → Seedance 2.5 Director

一句话：**先做导演决策，再把决策翻译成模型能看见的东西，最后才写 Prompt。**

编译以动作、部位表情、视线、空间、节奏、摄影与声音等可观察信息为核心。情绪标签可定位意图，但不能替代具体表演证据；这是写作原则 `[推论]`，不是模型无法理解情绪的能力断言。

## 表演任务优先路由

用户只要原文、强度/克制调整、情绪弧线或 N 秒表演测试时，直接读 `references/emotion-performance.md`，用 `scripts/emotion_library.py` 按需读取所选完整条目。走 S4 → 轻量 S5 可见性检查 → S6 → S7，不强制补故事、人物小传、台词、道具、参考图或停靠点。

`performance` 是独立测试或嵌入镜头的表演片段；`production` 是完整生产 Prompt，保留素材/镜头流程；`raw` 是逐字原文正文。用户要完整故事时仍走下列流水线。全流程共用同一表演模块：强度、克制与台词密度独立；高光不强制静止或收束；连续部位变化没有次数配额。用户及已确认内容优先于默认手法。

## 工作边界

- **做**：概念、故事开发、剧本、台词与表演、导演与分镜、参考资产计划、Seedance 2.5 Prompt 编译（参考生为主；文生 / 关键帧 / 宫格 / 首尾帧 / 编辑 / 延长）、连续性与质量检查、成片问题定位。
- **不做**：不调用模型 API，不生成图片（只产出图像简报）；不虚构模型参数；不替用户决定与任务无关的美术风格。
- **事实分级**（写任何模型能力时必须带标签）：`[官方]` 火山方舟指南原文 · `[第三方]` 有出处的外部经验 · `[推论]` 本 Skill 的设计推论 · `[未验证]` 未核实假设。能力表见 `references/seedance-2.5-capabilities.md`。

完整生产到 S5b–S7 时读取 `references/production-workflow.md`。该接口把既有素材、任务参数、上游版本和审阅记录绑定到每条 clip；文本草稿不冒充可提交生产包。

## 流水线

```
S1 资源读取 → S2 需求识别 → 故事开发（S3a 概念 ↔ S3b 故事 ↔ S3c 剧本，含关键场景试写）→ 有针对性地修订 → S4 表演外化 → S5 导演与分镜（含 S5b 参考资产）→ S6 Prompt 编译 → S7 检查
```

▮ = 停靠点（等用户决定）。**创作前端允许迭代探索，生产后端走稳定步骤。** S3a 概念、S3b 故事、S3c 剧本是三种成果类型：概念说为什么值得拍，故事组织人物与事件，剧本交付完整场景与台词。它们不是三道必经审批关卡：从哪一种进入、停几次，由 S2 识别的意图、材料成熟度与自主度决定（见下节）。开发可以从一句台词、一个人物或一个片段试写进入，再回流核对故事；试写不等于批准上游，也不等于可直接编译生产。

| 阶段 | 读什么 | 产出什么 | 停靠 |
|---|---|---|---|
| S1 资源读取 | `references/stage-1-intake.md` | 资产登记（按保存契约）+ 缺口清单 | |
| S2 需求识别 | 同上 §需求识别 ＋ `references/scene-parameters.md` §一 | 创作意图 · 材料成熟度与入口 · 锁定项 · 目标终点与排除项 · 自主度 / 确认 / 保存 · 任务类型 · clip 数 · **场景参数卡** | |
| S3a 概念 | `references/concept-generation.md` ＋ `references/stage-3a-concept.md` ＋ `references/research-to-craft.md` | 对话内创作判断 + 候选（数量由任务定，可附关键场景试写）；选定内容；按保存契约写 `01_concept.md` | ▮ 候选（用户要选方向时） |
| S3b 故事开发 | `references/stage-3b-story.md` ＋ `references/screenwriting-traditions.md` | 对话内可连续阅读的故事正文（辅助表按需）；按保存契约写 `02_story.md` + `ip.md` | 按执行契约 |
| S3c 剧本落地 | `references/stage-3c-script.md` ＋ `references/scene-parameters.md` | 对话内每场剧本页；按保存契约写 `03_script/scene-XX.md` | 按执行契约 |
| S4 表演外化 | `references/stage-4-performance.md` ＋ `references/emotion-performance.md` | 有序表演块（C 层）及内部核对记录 | |
| S5 导演与分镜 | `references/stage-5-directing-storyboard.md` ＋ `references/director-lenses.md` ＋ `references/camera-vocabulary.md` | `04_shots/scene-XX-clipYY.md` 分镜卡（五层） | ▮（与 S5b 一起） |
| S5b 参考资产 | `references/stage-5b-reference-assets.md` | `05_assets/asset-plan.md`：资产清单 + 图像简报 + 上传顺序 | 等用户回填 |
| S6 Prompt 编译 | `references/stage-6-prompt-compiler.md` ＋ `templates/prompt-templates.md` | `06_prompts/scene-XX-clipYY.prompt.md` | |
| S7 检查 | `references/stage-7-qa-continuity.md` ＋ `scripts/validate_prompt.py` | `07_qa/…`；有成片时追加 `references/validation-log.md` | |

**只读当前阶段需要的文件。** 执行契约判断一次后复用，后续不重复推导保存与范围。
**按类型叠加**：基调为动作 / 悬疑恐怖 / UGC 广告 / 蒙太奇时，S3b–S5 加读 `references/genre-packs.md` 对应一包。
**剧情创作共用**：`references/anti-mechanical.md` 与 `references/character-scene-development.md`（交流处境、人物声音、连续对话诊断）。追查因果时读 `references/causal-chain.md`；缺事实或语言材料时读 `references/research-to-craft.md`；重写时读 `references/creative-loop.md`，并查 `references/preference-ledger.md` 中用户确认过的偏好。表演测试以状态衔接替代剧情链，不为填表增加故事。

## 需求识别与执行

**S2 先读 `references/execution-contract.md`，它是范围、自主执行、用户确认、文件保存的唯一决策源。** 结合本次原文、历史授权及材料成熟度确定目标终点、排除项和入口；不依靠触发词重新裁决。同一任务换种说法不应扩大交付。复杂请求按 `templates/execution-record.json` 记录，用 `scripts/route_check.py` 的 `--record` 接口 检查内部一致性；脚本不验证语义理解。

| 用户意图 | 成果与入口 |
|---|---|
| 想故事 | S3a 创作判断与候选，必要时试写；用户要选方向时停 |
| 发展已有想法 | S3b，从人物、台词或片段长出完整故事 |
| 写完整剧本 | S3c；从零开始时按需要发展概念和故事，交付完整场景 |
| 局部改写 | 从现有稿及批评定位层级，只改授权范围 |
| 进入生产 | 已有剧本进 S4/S5；分镜转 Prompt 补资产后进 S6；到达所需成果即停 |
| 表演测试 | 原文、强度、弧线、定时测试走独立路由，不补故事 |
| 只保存 | 保存指定现有文本，不启动新的创作 |

材料按内容判断：有台词不等于剧本已完成；场景表不是故事正文的替代，也不是写剧本的门槛。成熟部分沿用，缺口只补影响本次工作的部分。用户新修改请求授权其范围内改写；其他硬锁保留。

场景、镜头、clip 分别约束叙事范围、摄影和生成预算，不互相推导。用户要“一场戏”不等于一个 clip，也不等于要 Prompt。用户要“写故事看看”只交故事；“一次跑完”只改变到目标为止的自主度。

对话交付默认不落盘；本次明确保存要求或已有项目保存约定决定写盘。确认内容不自动建立保存约定，自主推进不自动确认。停靠由用户需要的选择与不可替代缺口决定，不按阶段增加审批。生产 S5/S5b 的成本决策与素材回填按既有约定执行，已有授权不重复询问。

## 五层分离（贯穿 S5–S7）

| 层 | 名称 | 内容 | 读者 |
|---|---|---|---|
| A | 创作决策 | 为什么这样拍：意图、潜台词、观众此刻该知道什么 | 导演 / 编剧 |
| B | 分镜说明 | 供人阅读的镜头描述，可抽象 | 协作者 |
| C | 可观察信息 | 谁、在哪、面朝哪、做什么、看哪、说什么、镜头怎么动、光从哪来、什么声音、起止状态 | 编译器 |
| D | 最终 Prompt | 由 C 层按 Seedance 2.5 语法编译 | 模型 |
| E | 生产元数据 | 任务类型、素材编号与角色、ratio / duration / 格式、上游片段、透镜、版本、抽卡记录 | 生产管理 |

**只有 C 层进入 Prompt。** 已确认的有序表演块从 C 原样进入 D，保留先后、同步关系和末态。核对记录留在 E 层；不能以“外化”为由再次概括原文。

## 项目目录（一个 IP 一个目录，一个故事一个子目录）

```
<workspace>/<ip-slug>/
  ip.md                    世界观 · 人物总表 · 地点总表 · 视觉声音总则（跨故事共享）
  assets/assets.md         参考资产登记表（稳定资产 ID，上传编号按 clip 映射）+ 文件
  <story-slug>/            一个故事 / 一条先导 / 一集
    00_brief.md · 01_concept.md · 02_story.md
    03_script/scene-XX.md
    04_shots/scene-XX-clipYY.md
    05_assets/asset-plan.md
    06_prompts/scene-XX-clipYY.prompt.md
    07_qa/scene-XX-clipYY.qa.md
    project-state.json       当前版本、确认快照、依赖、未决项（保存项目按需）
```

ip-slug = 用户给的 IP 名或概念选定后的标题；story-slug = 格式 + 标题（`teaser-30s-举证`、`ep01-…`）。用户已有目录时沿用。已保存项目的版本、快照、恢复、正典和 `.production.json` 位置见 `references/project-state.md`；只读检查用 `scripts/project_check.py`。模板在 `templates/`：`ip.md` · `story.md` · `script-scene.md` · `shot-card.md` · `reference-asset-brief.md` · `asset-registry.md` · `prompt-templates.md`。

一个 **clip = 一次 Seedance 2.5 生成 ≤ 30 秒**。clip 衔接策略在 S5 决定（延长 / 尾帧接首帧 / 独立）。

示例：`examples/example-01-kitchen-keys.md`（完整走查）· `examples/example-02-one-scene-three-lenses.md`（透镜对照）· `examples/example-03-yogurt-comedy.md`（喜剧语域）· `examples/concept-worked-examples.md`（概念协议，概念模式不读）。示例展示流程，不提供答案，禁止复用其中的查询、候选、台词。

## 硬规则

1. **不虚构模型能力。** 写能力 / 参数必须带事实标签；`[未验证]` 项只能作为可选尝试并注明。
2. **完整生产以图 + 文为核心。** production 的 S6 前做 S5b 资产计划，无参考图的跨 clip 一致性标高风险；用户选择文生时走 T1。performance/raw 不要求资产计划。
3. **素材编号按每条 clip 的实际上传顺序绑定，明确各素材参考范围。** production 开头有【素材绑定】，无素材写明；实际引用都必须已登记，不虚构图或视频。不靠图片文字绑定角色。`[官方]` 同一素材可承担明确兼容的参考用途；不为凑模板复制或拆图。
4. **各层时间分别核对。** production 镜头时间线或独立 performance 时间线从 0 连续到 duration；镜内节拍不能跨镜，不能把镜头与节拍时长相加。使用整数秒的本地编译约定，不以时间戳精确控动作频次。
5. **画面对象优先正向描述。** “不眨眼”等行为保持与排除对象不是同类；不能据一个否定词改写原文。能力边界查能力表，静态脚本不判任意否定语义。
6. **台词逐字加引号、标注说话人与语言、给时间窗；非说话者写嘴部状态。** `[官方示例] + [第三方]`
7. **表演有可见证据。** 保留速度、幅度、渐变与控制信息；删空泛替代，不按词性删细节。原文不润色，已有台词不因本规则被擅改。
8. **每个镜头写景别、清晰可执行的运镜与起止状态。** 冷门术语“术语 + 描述”。`[官方]` 复合运镜说明同步或先后；单一运镜只是降低复杂度的本地建议。
9. **每个 clip 的 Prompt 自足。** 外观锁、空间、光源、声音在每个 clip 重写。`[推论]`
10. **手法服务需求。** 复用、重复、持续或创新按本次目标判断，不以配额或词频裁决。导演名字不作为风格捷径进入 Prompt；通用技术名如希区柯克变焦可配可见描述使用，透镜不覆盖锁定表演。
11. **创作审阅看具体文本**：剧情可检查观众记住的画面、人物选择与理解变化，不要求每片都有反转、物件、走位或固定数量的候选与场次；这些方法保留为可选工具。表演检查高光、衔接、可见性与保真。
12. **想故事时走概念协议**（`references/concept-generation.md`）：创作判断 → 默认地图 → 按任务选入口 → 研究服务缺口 → 候选发展（数量由任务定，候选之间在人物、关系、事件后果或观众理解上真正不同）→ 可选关键场景试写。用户要选方向时不预选；范围、确认与保存遵循执行契约；示例的查询与候选禁止复用；故事依赖的现实事实有研究记录（发现 / 可信范围 / 影响的决定），未锁定的虚构可以创造；选定后跑 `scripts/validate_concept.py`（只查格式）。
13. **按材料成熟度进入，允许关键场景先行与回流。** 从零开始时故事先于剧本与分镜；已有材料和明确的单阶段/表演测试不重复补上游，不强加停靠。试写、剧本、表演、分镜中的发现可按 `references/creative-loop.md` 回到上游，不越过用户锁定内容；重写先诊断层级、保留上一版、说明改了什么并核对失去的细节；达到任务目标后停止，不为"再高级一点"重写。
14. **S6 后分层校验**：production 默认 `python3 scripts/validate_prompt.py <prompt.md>`；片段加 `--artifact performance --duration N`，原文加 `--artifact raw --entry-id N`。可选记录接口见 `references/performance-record.md`。完整生产包另按 `references/production-workflow.md` 执行 `--production-record … --require-ready`，不得把基础 CLI 的零错误称为生产就绪。未落盘可用临时文件检查，不强制保存产物。ERROR 修复；WARN 审阅；脚本通过不等于表演/成片通过。
15. **需求参数优先于默认美学。** 场景强度、角色情绪强度、克制、方向与台词密度分别判断；高强度可以内收且无台词，不自动套预设。
16. **状态与因果连贯。** 剧情检查前因后果，表演检查状态推进/持续。删除测试不能删识别性细节；同步多部位不等于多个无关任务（`references/causal-chain.md`）。

## 默认输出契约

停靠点交付摘要与必要的选择问题。performance/raw 按表演模块交付；production 到达 S6 交付：

1. 需求识别结果（一行：意图 / 任务类型 / 运行模式 / clip 数 / 锁定判定）。
2. 实际保存的产物路径（未落盘不虚构路径）。
3. 每个 clip 的最终 Prompt（代码块）+ 参数建议表（content.role / ratio / duration / 输出格式）+ 校验结果摘要。
4. 未验证假设与抽卡风险点（来自 S7）。
5. 下一步：用户需要提供什么（参考图回填 / 确认 / 成片反馈）。

## 快速路由

- “帮我构思短片”：到 S3a，不因“短片”一词扩大到剧本。
- “故事原文写成剧本”：故事成熟则进 S3c，不因“原文”转成表演测试。
- “只拆分镜，不要 Prompt”：S4 → S5，终点 S5，排除 S6；按已有素材需要附资产缺口。
- “不保存，直接出 Prompt”：到 S6，对话交付，临时文件可做既有校验。
- “只保存已有稿”：保存动作，不新编故事或剧本。
- “一场 90 秒戏”：一个叙事场景，镜数与 clip 数另定。
- “改结尾，其他台词不动”：改授权结尾与直接影响上下文，保留其余硬锁。

具体状态与异常判断统一见 `references/execution-contract.md`。不确定且结果会实质不同时只问必要问题，否则写明假设并推进已授权工作。
