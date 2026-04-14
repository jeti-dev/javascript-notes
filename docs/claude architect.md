---
title: Claude Architect
layout: default
---

# Claude Architect

## Claude Code in action

Source: [https://anthropic.skilljar.com/claude-code-in-action](https://anthropic.skilljar.com/claude-code-in-action)

- coding assistants use LLMs to complete tasks
  - however, an LLM can't do much on its own, it needs tools to e.g. read the content of a file -> coding assistants can work with tools to help the work of the LLM
  - how it works: When I send a propmpt, the assistans appends extra text like "if you want to read a file, response with ReadFile: name-of-file". If the LLM wants to read a specific file, it would return that "tool" so the assistant can send the content of that file.
- `/init`: info about your project -> CLAUDE.md
  - CLAUDE.local.md -> personal instructions
  - \~/.claude/CLAUDE.md -> global instructions, applied every time
- memory mode: type # in the chat and if you add a message, that will be added to CLAUDE.md and it will be referenced by the assistant
- use @ to mention files in the chat or md files
- the bigger the context, the less precise the LLM works
  - you should list important files in the CLAUDE.md file so they'd be picked up automatically when needed
- boost intelligence:
  - use plan mode for complex tasks where there is broad topic or lots of files (multi-step tasks)
  - or enable thinking: think, think more, ..., Ultrathink; use for focused, narrow tasks (complex tasks)
- when context starts drifting: e.g. Claude is writing tests but it has to fix a missing package error. Before moving to the next tests, you can select from which point of the conversation you want to continue.
  - /compact: it summarizes the current conversion to keep the context
  - /clear: start from scratch
- .claude/commands/my-command.md: custom commands, called with /my-command
  - $ARGUMENTS to send arguments
- MCP: extend the capabilities e.g. use new tools
- /install-github-app
  - adds 2 GH actions: PR request + mention @claude in an issue or PR
    - e.g. mention @claude in a new issue to check sg. e.g. a button is not working as intended
  - files in the .github folder
  - action file:
    - custom_instructions -> instructions for Claude
    - we have to list all available tools
- SDK: run Claude programatically with Typescript, Python or via the CLI
  - by default, it can only read files -> write tools must be explicitly set
  - useful in hooks

### Hooks - like lifecycle hooks

- list of hooks:
  - Notification - Runs when Claude Code sends a notification, which occurs when Claude needs permission to use a tool, or after Claude Code has been idle for 60 seconds
  - Stop - Runs when Claude Code has finished responding
  - SubagentStop - Runs when a subagent (these are displayed as a "Task" in the UI) has finished
  - PreCompact - Runs before a compact operation occurs, either manual or automatic
  - UserPromptSubmit - Runs when the user submits a prompt, before Claude processes it
  - SessionStart - Runs when starting or resuming a session
  - SessionEnd - Runs when a session ends
  - PreToolUse (can block a tool use if it returns an error)
  - PostToolUse
- the stdin and tool_input of my commands depend on the type of hook being executed
- add config to one of the `settings.json`

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read|Grep",
        "hooks": [
          {
            "type": "command",
            "command": "node /home/hooks/read_hook.ts"
          }
        ]
      }
    ]
  }
}
```

```json
// The parameter we get in our hook command
{
  "session_id": "2d6a1e4d-6...",
  "transcript_path": "/Users/sg/...",
  "hook_event_name": "PreToolUse",
  "tool_name": "Read",
  "tool_input": {
    "file_path": "/code/queries/.env"
  }
}
```

```javascript
// An example hooke file that throws an error if Claude wants to read our .env file.
async function main() {
  const chunks = [];
  for await (const chunk of process.stdin) {
    chunks.push(chunk);
  }

  const toolArgs = JSON.parse(Buffer.concat(chunks).toString());

  // Extract the file path Claude is trying to read
  const readPath =
    toolArgs.tool_input?.file_path || toolArgs.tool_input?.path || "";

  // Check if Claude is trying to read the .env file
  if (readPath.includes(".env")) {
    console.error("You cannot read the .env file");
    process.exit(2);
  }
}
```

- advanced hook: we can ask Claude to do sg e.g. we can tell it to check some files specifically for implementations of useful stuff in case Calude wants to create a new file with redundant code
