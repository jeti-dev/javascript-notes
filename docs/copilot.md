---
title: Copilot
layout: default
---

# Copilot

1. custom instructions: project.wide coding standards and architectural context
   - `/init` to generate a base file
2. prompt files: reusable prompts for recurring tasks (e.g. reviews, scaffolding)
   - `/prompts`
3. custom agents: specialized workflows or personaes (e.g. TDD, security audit)
   - `/agents`
4. agent skills: domain specific capabilities
   - `/skills`

## Quick reference

1. If AI is not set up in your repo, run `/init` first. It'll generate a `copilot-instructions.md` file.
2. Add targeted instructions in `*.instructions.md` files.
3. Automate repetitive tasks: create prompt files, connect to MCP servers
4. Create specialized workflows: create custom agents, agent skills.
5. apply coding standards everywhere: always-on instructions
6. different rules for different filetypes: file based instructions
7. reusable tasks: prompt files
8. specialized AI persona with tool restrictions: custom agents
9. multi step workflow with scripts: agent skills
10. connect to external APIs or DBs: MCP
11. automate tasks at agent lifecycle points: hooks

## Compare Models

- https://docs.github.com/en/copilot/reference/ai-models/model-comparison

## Instructions

- always-on: `.github/copilot-instructions.md`
  - one or more`AGENTS.md` (for agents) files to be applied to all agents
- properties: name, description, applyTo
- `applyTo` property: to which filetype the instruction should be applied to
- reference other files with `[your text](./your-path.md)`
- location: `.github/instructions`

## Prompt files

- or "slash commands"
- call them via the chat
- usage: scaffolding, running and fixing tests, preparing a pull request; oerrida the default behavior of a custom agent
- location: `.github/prompts`
- properties: name, description, argument-hint, agent, model, tools
- variables: e.g. ${workspaceFolder}, ${selection}, ${file}

## Agents

- pre-configured: Agent, Plan, Ask
- custom agent: I create them
- subagent: runs in the background, multiple can be ran parallel
- handoff: transferring a session from one agent type to another; use the `handoffs` property
- agent types: local, background (uses git worktrees, has limitations), cloud, third-party
- properties: name, description, argument-hint, tools, agents, model, user-invokable, disable-model-invocation, infer (deprecated), target, mcp-servers, handoffs (label, agent, propmpt, send, model),

### Agent Skills

- instructions, scripts and resources that can be loaded when relecant tasks are performed
- it is an open standard, works across AI tools
- location: `.github/skills`
- each skill must have a dedicated folder and a `SKILL.md` file in it
- properties: name, description, argument-hint, user-invokable, disable-model-invokation
- what to include in the body:
  - What the skill helps accomplish
  - When to use the skill
  - Step-by-step procedures to follow
  - Examples of the expected input and output
  - References to any included scripts or resources
- skills are also available as slash commands
- Copilot automatically discovers and injects skills
- https://github.com/github/awesome-copilot
- https://github.com/anthropics/skills

### Agent Tools

- types: built-in, MCP tools, extension tools
- the agent can select a tool on its own or we can tell it what to use e.g. `"Summarize the content from #fetch https://code.visualstudio.com/updates"`
- syntax: `#toolName`
- tool set: a group of tools e.g. `#edit`
  - we can write custom toolsets too
- list tools in chat: `#`

### Subagents

- usually invoked by other agents
- but we can start them with `#runSubagent`
- we can also explicitly say to "run a subagent to..."
- we can orchestrate them with a main orchestrator agent e.g.
  - "use the Planner agent to break down a feature into tasks"
  - "use the Architect agent to validate the tasks"
- anothe rtechnique: multi-perspecitve code review: we tell a main agent to run multiple subagents in parallel to check the code from different perspectives e.g. correctness reviewer and security reviewer

## MCP

- TODO

## Hooks

- TODO
