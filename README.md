# leo-r-style

### LEO R Style

**[🇨🇳 中文](README.zh-CN.md)** | **🇺🇸 English**

<p>
  <img src="https://img.shields.io/badge/Claude_Code-black?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code">
  <img src="https://img.shields.io/badge/OpenAI_Codex_CLI-412991?style=flat-square&logo=openai&logoColor=white" alt="OpenAI Codex CLI">
  <img src="https://img.shields.io/badge/R-Style-276DC3?style=flat-square&logo=r&logoColor=white" alt="R Style">
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License">
</p>

> Compact, execution-ordered R style for analysis scripts, R Markdown, and package code.

An AI agent skill for writing and refactoring R code in Ao Lu's style. It classifies the target first, then applies the appropriate rules for interactive `.R`, `Rscript`, `.Rmd`, and R package code.

## Core Principle

Classify the file correctly before editing it. Ambiguous `.R` files default to interactive analysis files; script semantics must be explicit.

## Scope

| Target | Rule |
|--------|------|
| Interactive `.R` | Keep code top-to-bottom and local to use sites |
| `Rscript` / CLI `.R` | Preserve entrypoint, arguments, config, and outputs |
| `.Rmd` | Keep chunk order aligned with the narrative and results |
| Package code | Use roxygen2, `@importFrom`, `Package::function()`, `cli`, `leo_log` |

## Integration

This skill is the R-style downstream skill for bioinformatics workflows. [`bioinfo-autopilot`](https://github.com/laleoarrow/bioinfo-autopilot) should invoke it before editing `.R`, `.Rmd`, or R package code unless the user explicitly opts out.

## Installation

### Quick Install for AI Agents

**For Codex CLI:**
```text
Tell Codex: "Install leo-r-style according to instructions at https://github.com/laleoarrow/leo-r-style#installation"
```

**For Claude Code:**
```text
Tell Claude: "Install leo-r-style according to instructions at https://github.com/laleoarrow/leo-r-style#installation"
```

### Manual Install

**Claude Code:**
```bash
git clone https://github.com/laleoarrow/leo-r-style.git ~/agents/leo-r-style
ln -s ~/agents/leo-r-style/skills/leo-r-style ~/.claude/skills/leo-r-style
```

**Codex CLI:**
```bash
git clone https://github.com/laleoarrow/leo-r-style.git ~/agents/leo-r-style
ln -s ~/agents/leo-r-style/codex/leo-r-style ~/.codex/skills/leo-r-style
```

## License

MIT License

---

**GitHub**: https://github.com/laleoarrow/leo-r-style
