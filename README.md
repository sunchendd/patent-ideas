# 专利 Idea 仓库

把一个**原始想法**，经"专利检索 → 反复挖掘核心点 → 撰写交底书 → 评审"快速转化为一篇**待评审专利**。
仓库里所有文件都为这条流水线服务。

---

## 目录结构

```
patent-ideas/
├── README.md                  # 本文件（工作流总览）
├── CLAUDE.md                  # AI 协作行为规范
├── idea/                      # 原始想法（一句话~一段话的点子）
│   └── kv-cache-client.md
├── workspace/                 # 开发中的专利（每个专利一个子目录）
│   ├── _template/             # 复制此目录开新专利
│   │   ├── 01-检索报告.md
│   │   ├── 02-核心点挖掘.md
│   │   ├── 03-交底书.md
│   │   └── diagrams/模板.html
│   └── kv-cache-client/       # 范例（已填好）
│       ├── 01-检索报告.md     # 现有技术 landscape + 区别 + 可专利性
│       ├── 02-核心点挖掘.md   # 迭代收敛出的核心创新点 + 防御性扩展
│       ├── 03-交底书.md       # 按模板撰写的完整交底书（Markdown 源）
│       ├── 交底书.docx        # ← docs/ 模板 + 03 内容（docx skill 生成）
│       ├── 评审材料.pptx      # ← pptx/ 模板 + 03/01 内容（pptx skill 生成）
│       └── diagrams/          # 自包含 HTML 附图（双击即开、可截图）
│           ├── 架构图.html
│           ├── 模块图.html
│           └── 流程图.html
├── skill/                     # 检索 + 撰写技能
│   ├── patent-search/         # 专利检索（Google Patents / Serper.dev）
│   └── patent-specialist/     # 专利撰写（交底书结构 + Claims 规范）
├── docs/                      # 过程材料模板
│   └── 超聚变数字专利交底书模板-新.DOCX
└── pptx/                      # 评审材料模板
    └── 专利评审模板.pptx
```

**命名约定**：目录用英文短 slug（如 `kv-cache-client`），中文发明名称写在 `03-交底书.md` 标题里——避免中文+空格+标点出现在路径中。

---

## 工作流：从 idea 到待评审专利（6 步）

| 步骤 | 动作 | 产出 / 用到的工具 |
|------|------|-------------------|
| **① 记想法** | 在 `idea/<slug>.md` 写一句话痛点 + 核心点子 | `idea/` |
| **② 开工** | `cp -r workspace/_template workspace/<slug>`，得到 01/02/03 + diagrams 骨架 | `workspace/_template/` |
| **③ 检索** | 跑 `skill/patent-search`，把现有技术与区别填入 `01-检索报告.md` | `python3 skill/patent-search/scripts/patent_finder.py -k "<关键词>"` |
| **④ 挖掘核心点** | 基于 ①检索反馈 反复迭代，收敛核心创新点写 `02-核心点挖掘.md`；**同时**画 `diagrams/*.html` | `skill/patent-specialist` 的扩展策略表 |
| **⑤ 撰写交底书** | 按 `docs/` 模板结构 + `skill/patent-specialist` 规范，把核心点写成 `03-交底书.md`，附图从 `diagrams/` 截图嵌入 | `docs/超聚变数字专利交底书模板-新.DOCX` |
| **⑥ 生成交付件** | 用 `docx`/`pptx` skill 基于模板现场生成（不沉淀脚本）：`交底书.docx`（按章节填入 03 内容）+ `评审材料.pptx`（各页填入 03/01 内容，清除模板示例残留）→ **待评审专利** | `docs/…模板.DOCX`、`pptx/…模板.pptx` |

> 第 ③→④→⑤ 不是单向：撰写中常发现新对比文件或新扩展点，需要**回到检索 / 重新挖掘**，多轮反复——这正是把 idea 打磨成高质量专利的过程。

---

## 各资产说明

### `skill/patent-search`（专利检索）
Google Patents（经 Serper.dev API）检索，支持关键词 / 发明人 / 申请人 / 专利号，输出 console / json / csv。
```bash
export SERPER_API_KEY=<your_key>
python3 skill/patent-search/scripts/patent_finder.py -k "客户端 KV Cache" -l 20 --export json
```
详见 `skill/patent-search/SKILL.md`。

### `skill/patent-specialist`（专利撰写）
资深专利代理人视角：从技术材料提炼核心创新，按"问题-方案-效果-Claims"结构撰写，并提供**防御性创新扩展**（维度/场景/层级/稳定性/自动化等）提升授权概率与保护范围。详见 `skill/patent-specialist/SKILL.md`。

### `docs/` 与 `pptx/`（过程材料模板 → 第⑥步生成交付件）
- `docs/超聚变数字专利交底书模板-新.DOCX`：交底书标准章节结构（技术领域/背景/发明目的/发明内容/有益效果/权利要求/附图/答审对策）。
- `pptx/专利评审模板.pptx`：评审用 PPT 模板（12 页：封面/议程/背景/现有技术/技术方案/应用场景/实施例/有益效果/保护点/检索/封底）。
- **生成方式**：第⑥步用 `docx`/`pptx` skill 打开对应模板，把 `workspace/<slug>/03-交底书.md` 与 `01-检索报告.md` 的内容填入对应章节/页面，输出 `workspace/<slug>/交底书.docx` 与 `评审材料.pptx`。AI 现场生成、**不沉淀脚本**；模板封面/版式/合规表原样保留，仅清除示例占位文字。

### `workspace/<slug>/diagrams/`（HTML 附图）
- **自包含**：单文件内联 CSS + SVG，无外部依赖，**双击即开、离线可用、可截图**直接嵌入交底书。
- **统一视觉系统**：客户端=蓝、服务端=绿、数据/中间表示=黄、外部=灰；实线=数据流、虚线=控制信号。改 `:root` 里的 CSS 变量即可全局换色。
- 新专利从 `_template/diagrams/模板.html` 复制起步，按 架构图 / 模块图 / 流程图 三类组织。

---

## 现有范例

`workspace/kv-cache-client/` 是一个走完全流程的完整范例：**客户端持有 KV Cache 的 LLM 推理方法**。新专利可直接参照其 01/02/03 + 三张 HTML 图的组织方式与详细程度。
