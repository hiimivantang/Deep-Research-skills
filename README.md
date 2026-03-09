# Deep Research Skill for Claude Code / OpenCode / Codex

[English](README.md) | [中文](README.zh.md)

> If you find this project helpful, please give it a star! :star:

> Inspired by [RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context](https://arxiv.org/abs/2511.18743)

A structured research workflow skill for Claude Code, OpenCode, and Codex, supporting two-phase research: outline generation (extensible) and deep investigation. Human-in-the-loop design ensures precise control at every stage.

![Deep Research Skills Workflow](workflow.png)

## Use Cases

- **Academic Research**: Paper surveys, benchmark reviews, literature analysis
- **Technical Research**: Technology comparison, framework evaluation, tool selection
- **Market Research**: Competitor analysis, industry trends, product comparison
- **Due Diligence**: Company research, investment analysis, risk assessment

## Installation

### Claude Code (Plugin - Recommended)

Install directly as a Claude Code plugin via repo link:

```
/plugin install https://github.com/Weizhena/deep-research-skills
```

Or test locally during development:

```bash
git clone https://github.com/Weizhena/deep-research-skills.git
claude --plugin-dir ./deep-research-skills
```

**Required**: Install Python dependency:
```bash
pip install pyyaml
```

### Claude Code (Manual)

<details>
<summary>Click to expand manual installation</summary>

```bash
git clone https://github.com/Weizhena/deep-research-skills.git
cd deep-research-skills

# English version
cp -r skills/research/* ~/.claude/skills/research/
cp -r skills/research-deep/* ~/.claude/skills/research-deep/
cp -r skills/research-add-items/* ~/.claude/skills/research-add-items/
cp -r skills/research-add-fields/* ~/.claude/skills/research-add-fields/
cp -r skills/research-report/* ~/.claude/skills/research-report/

# Chinese version (from variants/)
# cp -r variants/research-zh/* ~/.claude/skills/

# Required: Install agent and modules
cp agents/web-search-agent.md ~/.claude/agents/
cp -r agents/web-search-modules ~/.claude/agents/

# Required: Install Python dependency
pip install pyyaml
```

</details>

### OpenCode (default: gpt-5.4)

<details>
<summary>Click to expand OpenCode installation</summary>

```bash
git clone https://github.com/Weizhena/deep-research-skills.git
cd deep-research-skills

# Skills
cp -r skills/* ~/.claude/skills/   # or variants/research-zh for Chinese

# Required: Enable web search for current shell
export OPENCODE_ENABLE_EXA=1

# Optional: make it permanent
echo 'export OPENCODE_ENABLE_EXA=1' >> ~/.bashrc
source ~/.bashrc

# Required: Install agent and modules
cp agents/web-search-opencode.md ~/.config/opencode/agents/web-search.md
cp -r agents/web-search-modules ~/.config/opencode/agents/

# Required: Install Python dependency
pip install pyyaml
```

> **Important**: In OpenCode, ANY model's websearch requires `OPENCODE_ENABLE_EXA=1`. A plain `export` only affects the current shell; writing it to `~/.bashrc` makes it persistent. Without it, you only get `web fetch`, which is weaker for the deep research phase.

</details>

### Codex

<details>
<summary>Click to expand Codex installation</summary>

```bash
git clone https://github.com/Weizhena/deep-research-skills.git
cd deep-research-skills

# English version
mkdir -p ~/.codex/skills ~/.codex/agents
cp -r variants/research-codex-en/* ~/.codex/skills/

# Chinese version
# cp -r variants/research-codex-zh/* ~/.codex/skills/

# Required: Install web researcher agent and modules
cp agents-codex/web-researcher.toml ~/.codex/agents/
cp -r agents-codex/web-search-modules ~/.codex/agents/

# Required: Install Python dependency
pip install pyyaml
```

Add or update `~/.codex/config.toml` using either method below:

**Option A: Automatic script**

```bash
cd deep-research-skills
bash scripts/install-codex.sh
```

**Option B: Manual edit**

```toml
suppress_unstable_features_warning = true

[features]
multi_agent = true
default_mode_request_user_input = true

[agents.web_researcher]
description = "Use this agent when you need to research information on the internet, particularly for debugging issues, finding solutions to technical problems, or gathering comprehensive information from multiple sources. This agent excels at finding relevant discussions. Use when you need creative search strategies, thorough investigation, or compilation of findings from multiple sources."
config_file = "agents/web-researcher.toml"
```

</details>

## Commands

When installed as a **plugin**, skills are namespaced with `deep-research:`:

| Plugin Command | Description |
|------------------|-------------|
| `/deep-research:research` | Generate research outline with items and fields |
| `/deep-research:research-add-items` | Add more research items to existing outline |
| `/deep-research:research-add-fields` | Add more field definitions to existing outline |
| `/deep-research:research-deep` | Deep research each item with parallel agents |
| `/deep-research:research-report` | Generate markdown report from JSON results |

When installed **manually** (standalone), use without namespace: `/research`, `/research-deep`, etc.

> **Codex**: You can trigger these skills from `/skills` -> `List Skills`, or ask naturally, for example `Use the research skill to build an outline for AI Agent Demo 2025`.

## Workflow & Example

> **Example**: Researching "AI Agent Demo 2025"

### Phase 1: Generate Outline
```
/deep-research:research AI Agent Demo 2025
```
💡 **What will happen**: Tell it your topic → It creates a research list for you

**You get**: A list of 17 AI Agents to research (ChatGPT Agent, Claude Computer Use, Cursor, etc.) + what info to collect for each

### (Optional) Not satisfied? Add more
```
/deep-research:research-add-items
/deep-research:research-add-fields
```
💡 **What will happen**: Add more research items or field definitions

### Phase 2: Deep Research
```
/deep-research:research-deep
```
💡 **What will happen**: AI automatically searches the web for each item, one by one

**You get**: Detailed info for each Agent (company, release date, pricing, tech specs, reviews...)

### Phase 3: Generate Report
```
/deep-research:research-report
```
💡 **What will happen**: All data → One organized report

**You get**: `report.md` - A complete markdown report with table of contents, ready to read or share

## Need Help?

If you have questions, ask Claude Code, OpenCode, or Codex to explain this project:
```
Help me understand this project: https://github.com/Weizhena/deep-research-skills
```

## References

- RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context

## License

MIT
