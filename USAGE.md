# codemd — Usage Guide

**Code from Markdown**: detects ` ```bash ` / ` ```python ` code blocks in AI chat responses, executes them, and feeds results back to the model.

---

## Quick Start

```bash
cd ~/workspace/codemd
uv run python -m src.main agent-chat \
  --base-url http://localhost:1234/v1 \
  --api-key sk-xxx \
  --model Qwen3-Max \
  --allow-shell --allow-write \
  --auto-execute-code --no-tools --stream
```

Then just chat naturally. When the AI outputs a ` ```bash ` code block, it gets executed automatically.

---

## Key Features

### Code Block Detection
AI writes ` ```bash ` or ` ```python ` in its response → codemd extracts the code, executes it, and sends the output back to the AI. No function calling required.

### Multi-turn Context
Full conversation history is injected into every prompt. The AI remembers what was discussed even after 20+ turns.

### Dangerous Command Protection
Commands like `rm -rf`, `dd`, `mkfs`, `reboot` always require user confirmation, even in `--auto-execute-code` mode.

### Markdown Rendering
AI responses are rendered as formatted markdown using `rich` — **bold**, `code`, code blocks with syntax highlighting.

### Streaming
With `--stream`, tokens appear in real-time as the model generates them.

---

## Flags Reference

| Flag | Description |
|------|-------------|
| `--base-url` | API endpoint (e.g. `http://localhost:1234/v1`) |
| `--api-key` | API key |
| `--model` | Model name |
| `--allow-shell` | Allow shell command execution |
| `--allow-write` | Allow file write operations |
| `--auto-execute-code` | Execute code blocks without asking for confirmation |
| `--no-tools` | Skip sending function calling tool definitions to the model |
| `--stream` | Stream tokens in real-time |

### Recommended combinations

**Code-block-only mode (works with any model):**
```bash
--allow-shell --allow-write --auto-execute-code --no-tools --stream
```

**Full tool-calling mode (for models that support function calling):**
```bash
--allow-shell --allow-write --stream
```

---

## Examples

```bash
# Basic chat
uv run python -m src.main agent-chat \
  --base-url http://localhost:1234/v1 \
  --api-key sk-xxx \
  --model Qwen3-Max \
  --allow-shell --allow-write \
  --auto-execute-code --no-tools

# One-shot prompt (non-interactive)
uv run python -m src.main agent-chat \
  --base-url http://localhost:1234/v1 \
  --api-key sk-xxx \
  --model Qwen3-Max \
  --allow-shell --allow-write \
  --auto-execute-code --no-tools \
  "list files on my Desktop"
```

---

## Tips

- **Model choice**: Qwen3-Max and Qwen3-Coder work best. Kimi models may refuse to execute commands.
- **Conciseness**: AI responses can be verbose. Add `--append-system-prompt "回答控制在2句话以内"` for shorter replies.
- **Context retention**: The AI sees full conversation history. If it forgets context, it's a model limitation, not a bug.
