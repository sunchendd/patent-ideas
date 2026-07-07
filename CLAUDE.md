# 专利 Idea 仓库 — Claude 工作指引

本仓库把原始 idea 经"检索 → 挖掘 → 撰写 → 评审"快速转化为待评审专利。
通用编码规范（Think Before Coding / Simplicity / Surgical Changes / Goal-Driven）见上层 `CLAUDE.md`，本文件只讲本仓库专属事项。

## 目录与用途

- `idea/`       原始想法（一句话~一段话的点子）
- `workspace/`  开发中的专利，每个专利一个子目录；`_template/` 是可复制的空白骨架
- `skill/`      `patent-search`（专利检索）、`patent-specialist`（专利撰写）
- `docs/`       交底书模板（DOCX）
- `pptx/`       评审模板（PPTX）

## 驱动一个新 idea 的标准流程

1. `idea/<slug>.md` 记痛点 + 点子
2. `cp -r workspace/_template workspace/<slug>` 开工
3. `skill/patent-search` 检索 → 填 `01-检索报告.md`
4. 反复挖掘核心点 → `02-核心点挖掘.md`；同时画 `diagrams/*.html`
5. `skill/patent-specialist` + `docs/` 模板 → 撰写 `03-交底书.md`
6. 基于模板生成两个交付件（用 `docx`/`pptx` skill 现场生成，**不沉淀脚本**）：
   - `交底书.docx` ← `docs/超聚变数字专利交底书模板-新.DOCX` + `03-交底书.md` 内容
   - `评审材料.pptx` ← `pptx/专利评审模板.pptx` + `03`/`01` 内容（清除模板里的示例残留文字）

③→④→⑤ 是多轮反复，不是单向——撰写中常发现新对比文件或新扩展点，需要回到检索/重新挖掘。

## 约定

- 目录用英文短 slug；中文发明名称写进 `03-交底书.md` 标题。
- HTML 图必须自包含离线（内联 CSS + SVG，无 CDN），可截图嵌入交底书；视觉系统：客户端蓝 / 服务端绿 / 数据黄 / 外部灰。
- 检索报告**严禁编造专利号**；未检索到的字段留「待检索」占位，并给出可执行检索命令。
- 交底书是用户原文：迁移逐字保留，不擅自改写；发现编号/结构问题在 `02-核心点挖掘.md` 标注，提示用户核对。

完整说明见 `README.md`。
