<<<<<<< 
直接下载，设置好配置文件
到对应目录下运行
python-m wowcode
即可
=======
# WowCode

A terminal-based AI coding assistant — built with [Textual](https://github.com/Textualize/textual) for the TUI, supporting multiple LLM providers, extensible tools, sub-agents, and a permission system.

## Features

- **Multi‑Provider** — Anthropic Claude, OpenAI, and any OpenAI‑compatible API
- **Rich TUI** — Textual‑powered terminal UI with streaming, tool call visualization, and permission dialogs
- **Tool System** — built‑in tools (read file, edit file, bash, grep, glob, task management, sub‑agent dispatch) + MCP server integration
- **Sub‑Agent Orchestration** — spawn child agents with isolated worktrees or in‑process
- **Skill System** — extend behavior via `SKILL.md` files loaded at startup
- **Permission Layers** — 6 permission modes (from full‑auto to strict approval), dangerous command detection, path sandboxing
- **Hook System** — event‑driven automation on tool calls, permissions, agent turns, and more
- **Context Management** — automatic conversation compaction with recovery when approaching token limits
- **Memory & Instructions** — persistent user memory, project‑level instructions, and auto‑extraction
- **Team Mode** — multi‑agent coordination with in‑process, tmux, or iTerm2 spawning
- **Worktree Isolation** — git worktree isolation for parallel agent tasks
- **Sandbox Execution** — optional bubblewrap (bwrap) sandbox for command execution
- **Non‑Interactive Mode** — pipe a prompt via `-p` for CI/CD or scripting (`--output-format stream-json` for NDJSON streaming)
- **Remote Mode** — WebSocket server with browser UI (`--remote`)

## Quick Start

```bash
# Install
pip install wowcode

# Or with uv
uv tool install wowcode

# Set your API key
export ANTHROPIC_API_KEY=sk-...

# Run
wowcode
```

### Configuration

WowCode looks for `.wowcode/config.yaml` in the current directory. Minimal example:

```yaml
providers:
  - name: claude
    protocol: anthropic
    model: claude-sonnet-4-20250514
    base_url: https://api.anthropic.com/v1
```

Supported protocols: `anthropic`, `openai`, `openai-compat`.

### Usage

```bash
# Interactive TUI (default)
wowcode

# Non‑interactive: execute a prompt and output text
wowcode -p "What files are in this project?"

# Non‑interactive with structured JSON streaming
wowcode -p "Summarize main.py" --output-format stream-json

# Remote WebSocket mode (browser UI at http://localhost:18888)
wowcode --remote

# Run with a specific permission mode
wowcode --mode restricted
```

## Project Structure

```
wowcode/
├── __main__.py          # Entry point and CLI parser
├── app.py               # Textual TUI application
├── agent.py             # Core agent loop (event‑driven)
├── client.py            # LLM client abstraction (Anthropic / OpenAI)
├── config.py            # YAML configuration loader
├── conversation.py      # Conversation history management
├── prompts.py           # System prompt construction
├── driver.py            # Terminal driver (no‑alt‑screen)
│
├── agents/              # Sub‑agent system
│   ├── loader.py        # Agent loading from skill dirs
│   ├── fork.py          # Worktree‑based agent isolation
│   ├── task_manager.py  # Background task lifecycle
│   └── builtins/        # Built‑in agent definitions
│
├── commands/            # Slash‑command framework
│   ├── parser.py        # Command parsing
│   ├── registry.py      # Command registration
│   └── handlers/        # Built‑in command handlers
│       ├── help.py, clear.py, compact.py, rewind.py
│       ├── session.py, memory.py, status.py
│       ├── permission.py, sandbox.py
│       ├── plan.py, review.py, skill.py
│       ├── mcp.py, worktree.py, tasks.py
│       └── trace.py, skill_register.py
│
├── context/             # Context window management
│   └── manager.py       # Auto‑compaction & recovery
│
├── filehistory/         # File edit history tracking
├── hooks/               # Event hook system (engine, loader, models)
├── mcp/                 # MCP client & tool wrapper
├── memory/              # Persistent memory & instructions
│
├── permissions/         # Permission & sandbox system
│   ├── checker.py       # Permission request evaluation
│   ├── modes.py         # 6 permission modes
│   ├── rules.py         # Persistent allow/deny rules
│   ├── dangerous.py     # Dangerous command detection
│   └── sandbox.py       # Path sandboxing
│
├── sandbox/             # Command execution sandbox
│   ├── bwrap.py         # Bubblewrap integration
│   └── seatbelt.py      # macOS Seatbelt / sandbox-exec
│
├── skills/              # Skill system
│   ├── loader.py        # Skill discovery & loading
│   ├── executor.py      # Skill execution
│   └── builtins/        # Built‑in skills
│
├── teams/               # Multi‑agent team orchestration
│   ├── coordinator.py   # Team coordinator agent
│   ├── manager.py       # Team lifecycle management
│   └── spawn_*.py       # Backend spawners (in‑process, tmux, iTerm2)
│
├── tools/               # Tool system
│   ├── base.py          # Tool base class & streaming
│   ├── bash.py, read_file.py, write_file.py, edit_file.py
│   ├── grep.py, glob.py, ask_user.py, agent_tool.py
│   ├── synthetic_output.py
│   └── impl/
│       └── tool_search.py   # Dynamic tool discovery
│
├── worktree/            # Git worktree management
├── web_content.py       # Web content fetching via Playwright
├── remote.py            # Remote WebSocket server
├── validator.py         # Config validation
├── serialization.py     # Conversation serialization
├── cache.py             # Response caching
│
└── styles.tcss          # Textual TUI stylesheet
```

## Permission Modes

| Mode     | Description                                                       |
|----------|-------------------------------------------------------------------|
| accept   | Auto‑approve everything                                           |
| prompt   | Prompt for every operation (default)                              |
| plan     | Prompt with plan mode context                                     |
| restricted | Auto‑deny dangerous operations, ask for others                  |
| sandbox  | Sandbox all commands                                              |
| offer    | Offer dangerous ops, log others                                   |

## Skills

WowCode loads skills from `~/.wowcode/skills/` and the project's `.wowcode/skills/` directory. Built‑in skills include:

- **test** — test runner with file watcher
- **review** — code review agent
- **commit** — semantic commit message generator
- **backend-interview** — backend technical interview practice

## Development

```bash
# Clone & setup
git clone https://github.com/wuuu111/wowcode.git
cd wowcode
uv sync

# Run in development
uv run wowcode

# Run tests
uv run pytest

# Install from local
uv tool install -e .
```

### Requirements

- Python ≥ 3.11
- Dependencies: textual, anthropic, openai, pyyaml, pydantic, mcp, httpx, websockets

## License

MIT
>>>>>>> a7e2f93 (docs: add comprehensive README)
