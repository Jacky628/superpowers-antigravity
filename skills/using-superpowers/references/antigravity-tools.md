# Antigravity Tool Mapping

Skills are written using Claude Code tool names. Antigravity runs on Gemini-family tooling, so when a skill references one of these tools, use the Antigravity equivalent:

| Skill references | Antigravity equivalent |
|-----------------|------------------------|
| `Read` (file reading) | `view_file` |
| `Write` (file creation) | `write_to_file` |
| `Edit` (file editing) | `replace_file_content` / `multi_replace_file_content` |
| `Bash` (run commands) | `run_command` |
| `Grep` (search file content) | `grep_search` |
| `Glob` (search files by name) | `find_by_name` |
| `TodoWrite` (task tracking) | Task artifact (`task.md`) or inline checklist |
| `Skill` tool (invoke a skill) | Read the skill's `SKILL.md` via `view_file` using the exact path in `<skills>` |
| `WebSearch` | `search_web` |
| `WebFetch` | `read_url_content` |
| `AskUserQuestion` / `ask_user` | `ask_question` |
| `Task` tool (dispatch subagent) | `invoke_subagent` (see [Subagent support](#subagent-support)) |

## Subagent support

Antigravity supports subagents via the `invoke_subagent` tool (and follow-up communication via `send_message` / `manage_subagents`). Built-in subagent types include `self` (full capabilities inherited from parent agent) and `research` (read-only exploration).

When a skill says to dispatch a named agent type, invoke `self` via `invoke_subagent` with the full prompt from the skill's prompt template:

| Skill instruction | Antigravity equivalent |
|-------------------|------------------------|
| `Task tool (superpowers:implementer)` | `invoke_subagent` (`TypeName: "self"`) with the filled `implementer-prompt.md` template |
| `Task tool (superpowers:spec-reviewer)` | `invoke_subagent` (`TypeName: "self"`) with the filled `spec-reviewer-prompt.md` template |
| `Task tool (superpowers:code-reviewer)` | `invoke_subagent` (`TypeName: "self"`) with the filled `code-reviewer.md` template |
| `Task tool (superpowers:code-quality-reviewer)` | `invoke_subagent` (`TypeName: "self"`) with the filled `code-quality-reviewer-prompt.md` template |
| `Task tool (general-purpose)` with inline prompt | `invoke_subagent` (`TypeName: "self"` or `"research"`) with your inline prompt |

### Prompt filling

Skills provide prompt templates with placeholders like `{WHAT_WAS_IMPLEMENTED}` or `[FULL TEXT of task]`. Fill all placeholders and pass the complete prompt as `Prompt` in `invoke_subagent` (with `Model: "inherit"` unless requested otherwise). The prompt template itself contains the agent's role, review criteria, and expected output format.

### Parallel dispatch

Antigravity supports parallel subagent dispatch via the `Subagents` array in a single `invoke_subagent` call. When a skill asks you to dispatch multiple independent subagent tasks in parallel, pass all independent entries in `Subagents` together. Keep dependent tasks sequential, but do not serialize independent subagent tasks just to preserve a simpler history.

## Additional Antigravity tools

These tools are available in Antigravity and map naturally into workflow steps:

| Tool | Purpose |
|------|---------|
| `list_dir` / `view_file_outline` / `view_code_item` | Inspect directories, file symbols, and specific functions/classes |
| `ask_question` | Request structured multiple-choice input from the user |
| `manage_task` | Check status, send input, or kill background commands |
| `define_subagent` / `send_message` / `manage_subagents` | Define custom subagents, message running/idle subagents, or manage their lifecycle |
