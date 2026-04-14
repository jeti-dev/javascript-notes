---
title: Claude Architect
layout: default
---

# Claude Architect

## Claude Code in action

- coding assistants use LLMs to complete tasks
    - however, an LLM can't do much on its own, it needs tools to e.g. read the content of a file -> coding assistants can work with tools to help the work of the LLM
    - how it works: When I send a propmpt, the assistans appends extra text like "if you want to read a file, response with ReadFile: name-of-file". If the LLM wants to read a specific file, it would return that "tool" so the assistant can send the content of that file.
- `/init`: info about your project -> CLAUDE.md
    - CLAUDE.local.md -> personal instructions
    ~/.claude/CLAUDE.md -> global instructions, applied every time
- memory mode: type # in the chat and if you add a message, that will be added to CLAUDE.md and it will be referenced by the assistant
- the bigger the context, the less precise the LLM works
    - you should list important files in the CLAUDE.md file so they'd be picked up automatically when needed
- boos intelligence:
    - use plan mode for complex tasks where there is broad topic or lots of files
    - or enable thinking: think, think more, ..., Ultrathink; use for focused, narrow tasks
- when context starts drifting: e.g. Claude is writing tests but it has to fix a missing package error. Before moving to the next tests, you can select from which point of the conversation you want to continue.
    - /compact: it summarizes the current conversion to keep the context
    - /clear: start from scratch
- .claude/commands/my-command.md: custom commands, called with /my-command
    - $ARGUMENTS to send arguments
- MCP: extend the capabilities e.g. use new tools