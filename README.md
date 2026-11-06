# Your AppSec Program Already Knows How to Secure AI

*(It just doesn't know it yet)*

BSides Columbus · November 6, 2026 · Chris Linebrink, [Cline Security](https://clinesec.com)

## What's here

| | |
|---|---|
| [`slide-deck.pptx`](slide-deck.pptx) | The deck as presented |
| [The releases](#the-releases) | The rule pack and the skills catalog |

## The talk in one paragraph

AI changed how you run the program more than what the program is. Most of a traditional AppSec program already covers AI development, with an extension rather than a replacement. A few things are genuinely new. The hard part is not the controls, it's operating them with a team that didn't grow when the volume of code did. Deterministic where you can, AI where you must. Then put your engineering hat on and build the parts that are missing.

## The releases

### cline-ai-rules

**[github.com/ClineSec/cline-ai-rules](https://github.com/ClineSec/cline-ai-rules)** · MIT

OpenGrep rules for AI application code, MCP servers and agent config files, mapped to the OWASP Top 10 for LLM Applications 2026, the OWASP Top 10 for Agentic Applications and the OWASP MCP Top 10. They also work with Semgrep. Every rule carries OWASP ids and ships with fixtures showing what it catches and what it deliberately leaves alone, and the whole pack is run against deliberately vulnerable testbed projects.

Covers Python and TypeScript code on the OpenAI, Anthropic, LiteLLM, LangChain, Vercel AI SDK and MCP SDKs; Go MCP servers; Java on Spring AI and LangChain4j; C# on Semantic Kernel, Microsoft.Extensions.AI and the OpenAI .NET library; and the instruction and config files that steer coding agents (`CLAUDE.md`, `AGENTS.md`, Cursor rules, agent settings, hooks, MCP config).

```sh
git clone https://github.com/ClineSec/cline-ai-rules
opengrep scan --config cline-ai-rules/rules.yaml path/to/your/repo
```

The pack is in active development and rules still change as testing turns up false positives and misses. Pin a commit if you need stable results.

### cline-skills

**[github.com/ClineSec/cline-skills](https://github.com/ClineSec/cline-skills)** · MIT

Open-source Agent Skills for Claude Code, Codex and Cursor. Install the catalog once and the skills are available in every project on your machine. Every assessment skill runs read-only, executes nothing it finds, reports any in-repository text that tries to steer the result, and ends in a fixed-shape report a human reviews and signs.

| Skill | What it does |
|---|---|
| `security-review` | Reviews a repository, a release, or the changes since a commit for what someone could do to it, from outside the repository |
| `threat-model` | Builds a threat model by discovery instead of an interview, with STRIDE, the three LLM questions, and a color-coded architecture diagram |
| `ai-inventory` | Finds the AI in a repository (SDKs, models, endpoints, prompts, MCP servers, agent frameworks, vector stores) and writes it down as a CycloneDX AI-BOM |
| `agent-config-integrity` | Assesses the agent, IDE, MCP, hook and permission config in a repository. Flags unrestricted allowlists, hooks that fetch and run remote code, and instructions that change what agents report about security |
| `tprm-review` | Researches a vendor from the trust documents you downloaded and the public record, and reports where its account doesn't hold together, with a recommended verdict |
| `mcp-preflight` | Answers whether it's safe for a team to use an MCP server, with this data, in this agent, before it connects |
| `init-policy-relay` | Connects a repository to a policy repository so agents sync security guidance at session start and load only what each task needs |
| `policy-relay-distill` | Distills policy documents into that guidance and commits it to a policy repository |
| `skill-creator` | Creates or revises portable skills in a catalog |

```sh
curl -fsSL https://raw.githubusercontent.com/ClineSec/cline-skills/main/scripts/team-skills.sh \
  | sh -s -- install https://github.com/ClineSec/cline-skills.git
```

Skills stay at the installed version until you run the install again. Automatic updates at session start are opt-in, and the repo's own security review explains what that opts you into.

**PolicyRelay** is the two `policy-relay` skills together. Your security policy lives in a wiki. Developers don't read it, and their agents definitely don't. PolicyRelay distills it into short, task-specific guidance in a Git repository of your own, syncs it into each consumer repository at the start of every agent session, and the agent loads only what its task needs. Bundles are validated before they're swapped in, nothing in a bundle is executed, and a bad bundle leaves the last accepted guidance intact. macOS and Linux. It was a Cline Security product. It's open source because it only helps if it's everywhere.

### Also mentioned

**[DevSecOps in a Box](https://github.com/AppSecThings/dsoinabox)** wraps best-in-class open source scanners (secrets, SAST, SBOM, SCA, IaC) into a single scanner with normalized severity, one report, waivers, benchmarking and gating. Not part of the talk, just how the scanning in it was done.

## Steal this and make it better

That is, explicitly, the point. This is your call to action: put on your engineering hat and build the thing your program is missing. Fork anything here. Open issues and PRs on the release repos. If you build something better, tell me and I'll link it.

## License

Code in the release repos is MIT. The deck and the text in this repo are CC BY 4.0.
