# Antigravity (`agy`) Tool Mapping

Skills speak in actions ("dispatch a subagent", "create a todo", "read a file"). On Antigravity (`agy`) these resolve to the native tools below.

| Action / Skill reference | Antigravity equivalent |
|--------------------------|------------------------|
| Dispatch a subagent (`Subagent (general-purpose):` template) | `invoke_subagent` with a built-in `TypeName` — `self` for full-capability work, `research` for read-only (and `send_message` / `manage_subagents` for follow-up) |
| Task tracking ("create a todo", "mark complete") | a **task artifact** — `write_to_file` with `IsArtifact: true` and `ArtifactType: "task"` (see [Task tracking](#task-tracking)). **Not** `manage_task`, which manages background processes. |
| Invoke / load a skill | Read the skill's `SKILL.md` via `view_file` using the exact path in `<skills>` |
| Read a file (`Read`) | `view_file` (or `view_file_outline` / `view_code_item` for symbols) |
| Create or overwrite a file (`Write`) | `write_to_file` |
| Edit an existing file (`Edit`) | `replace_file_content` / `multi_replace_file_content` |
| Run shell commands (`Bash`) | `run_command` (and `manage_task` to check/interact with background commands) |
| Search file content / file names (`Grep` / `Glob`) | `grep_search` / `find_by_name` (`list_dir` for directory listing) |
| Web search / fetch URL (`WebSearch` / `WebFetch`) | `search_web` / `read_url_content` |
| Ask user structured questions (`AskUserQuestion`) | `ask_question` |

## Task tracking

Antigravity has **no todo tool** (`manage_task` manages background
processes — `list`/`kill`/`status`/`send_input` — it is *not* a checklist). When a
skill says to create a todo list or track tasks, maintain a **task artifact**: a
markdown checklist saved with `write_to_file` (`IsArtifact: true`,
`ArtifactMetadata.ArtifactType: "task"`), edited with `replace_file_content` /
`multi_replace_file_content` as you go.

At the start of any multi-step task, create the task artifact listing every step of
your plan. As you complete each step, edit the artifact to mark it done (`- [x]`).
If the plan changes, update the checklist. Keep it current — it is your source of
truth for what remains; once the conversation gets long, re-read it before starting
each step.
