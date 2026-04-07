# leo-r-style

### LEO R 风格

**[🇺🇸 English](README.md)** | **🇨🇳 中文**

<p>
  <img src="https://img.shields.io/badge/Claude_Code-black?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code">
  <img src="https://img.shields.io/badge/OpenAI_Codex_CLI-412991?style=flat-square&logo=openai&logoColor=white" alt="OpenAI Codex CLI">
  <img src="https://img.shields.io/badge/R-Style-276DC3?style=flat-square&logo=r&logoColor=white" alt="R 风格">
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License">
</p>

> 面向分析脚本、R Markdown 和 package code 的紧凑型 R 编码风格。

一个 AI agent skill，用于按 Ao Lu 的风格编写和重构 R 代码。它会先判定目标文件类型，再对交互式 `.R`、`Rscript`、`.Rmd` 和 R package code 应用对应规则。

## 核心原则

编辑前先判定文件类型。语义不明确的 `.R` 默认按交互式分析文件处理；只有脚本语义被明确标注时，才按 `Rscript`/CLI 处理。

## 适用范围

| 目标 | 规则 |
|------|------|
| 交互式 `.R` | 代码按执行顺序自上而下，逻辑尽量就地展开 |
| `Rscript` / CLI `.R` | 保留入口、参数、配置和输出语义 |
| `.Rmd` | chunk 顺序与叙事和结果保持一致 |
| Package code | 使用 roxygen2、`@importFrom`、`Package::function()`、`cli`、`leo_log` |

## 集成关系

这个 skill 是生信工作流里的 R 风格下游 skill。[`bioinfo-autopilot`](https://github.com/laleoarrow/bioinfo-autopilot) 在编辑 `.R`、`.Rmd` 或 R package code 前，应先调用它，除非用户明确选择不套用该风格。

## 安装

### 快速安装（告诉 AI Agent）

**Codex CLI:**
```text
告诉 Codex: "根据 https://github.com/laleoarrow/leo-r-style#installation 的说明安装 leo-r-style"
```

**Claude Code:**
```text
告诉 Claude: "根据 https://github.com/laleoarrow/leo-r-style#installation 的说明安装 leo-r-style"
```

### 手动安装

**CC Switch:**
```bash
git clone https://github.com/laleoarrow/leo-r-style.git ~/agents/leo-r-style
mkdir -p ~/.cc-switch/skills
ln -s ~/agents/leo-r-style/skills/leo-r-style ~/.cc-switch/skills/leo-r-style
```

**Claude Code:**
```bash
git clone https://github.com/laleoarrow/leo-r-style.git ~/agents/leo-r-style
ln -s ~/agents/leo-r-style/skills/leo-r-style ~/.claude/skills/leo-r-style
```

**Codex CLI:**
```bash
git clone https://github.com/laleoarrow/leo-r-style.git ~/agents/leo-r-style
ln -s ~/agents/leo-r-style/skills/leo-r-style ~/.codex/skills/leo-r-style
```

建议统一把 `skills/leo-r-style` 当作规范安装入口，这样各宿主读到的是同一份内容。

## License

MIT License

---

**GitHub**: https://github.com/laleoarrow/leo-r-style
