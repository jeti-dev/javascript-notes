---
title: Claude Architect v2
layout: default
---

[https://anthropic-partners.skilljar.com](https://anthropic-partners.skilljar.com)

- AI fluency: framework & foundations
- Claude 101
- Claude Code 101
- Claude Code in action
- Introduction to agent skills
- Introduction to MCP
- Building with the Claude API
- MCP: advanced topics

# AI fluency: framework & foundations

- there is a vocabulary file!
- AI Fluency means engaging with AI in ways that are effective, efficient, ethical, and safe
- There are three primary ways we engage with AI:
  - Automation: AI executes specific tasks based on your instructions
  - Augmentation: You and AI collaborate as creative thinking and task execution partners
  - Agency: You guide AI to work independently on your behalf, shaping its knowledge and behavior rather than specific actions
- The AI Fluency Framework consists of four core competencies (the 4Ds):
  - Delegation: Deciding what work to do with AI vs. yourself
  - Description: Communicating effectively with AI systems
  - Discernment: Evaluating AI outputs critically
  - Diligence: Ensuring responsible AI collaboration
- Generative AI creates new content (text, images, code) rather than just analyzing existing data
- Modern systems like LLMs were made possible by three key developments:
  - Algorithmic and architectural breakthroughs (especially the transformer architecture)
  - Vast amounts of digital training data
  - Dramatic increases in computational power

## Delegation

- Delegation is about making thoughtful decisions about what work to do yourself, what to do together with AI, or what to let AI handle independently, and how to distribute those tasks.
- Problem Awareness means clearly understanding your goals and the nature of the work before involving AI.
- Platform Awareness involves understanding the capabilities and limitations of different AI systems.
- Task Delegation is the process of thoughtfully distributing work between humans and AI to leverage the strengths of each.
- Effective delegation requires both domain expertise and an understanding of AI capabilities.
- The goal isn't to automate everything, but to create the most effective human-AI partnership for any given task or goal.

## Description

- Description is about communicating with AI in ways that create a productive collaborative environment
- Product Description involves clearly defining what you want in terms of outputs, format, audience, and style
- Process Description guides how the AI approaches your request, which can be as important as specifying the end goal
- Performance Description defines behavioral aspects like whether the AI should be concise or detailed, challenging or supportive
- AI systems are interactive partners, not databases or vending machines
- Clear communication up front saves time and leads to better results

### Effective prompting

- Effective prompting combines clear communication principles with AI-specific techniques
- Six foundational prompting techniques:
  - Give context: Be specific about what you want, why you want it, and relevant background
  - Show examples: Demonstrate the output style or format you're looking for
  - Specify constraints: Clearly define format, length, and other output requirements
  - Break complex tasks into steps: Guide the AI through multi-step reasoning
  - Ask the AI to think first: Give space for the AI to work through its process
  - Define the AI's role or tone: Specify how you want the AI to communicate
- The "secret weapon": Ask the AI itself to help improve your prompt
- Successful prompting is iterative (and perhaps also collaborative with the AI!). Expect to refine your approach based on results
- Common successful patterns include providing clear task overviews, format specifications, explicit constraints, and relevant background information

## Discernment

- Discernment is your ability to thoughtfully evaluate what AI produces, how it produces it, and how it behaves
  - Product Discernment focuses on evaluating the quality of actual outputs (accuracy, appropriateness, coherence, relevance)
  - Process Discernment involves assessing how the AI arrived at its output, looking for logical errors, attention gaps, or inappropriate reasoning
  - Performance Discernment evaluates how the AI behaves within the collaboration process itself, considering whether its communication style is effective for your needs
- Discernment works hand-in-hand with Description in a continuous feedback loop
- Even the most advanced AI systems benefit from human judgment and oversight

## Diligence

- Diligence is about taking responsibility for our AI collaborations
- Creation Diligence involves being thoughtful about which AI systems we use and how we engage with them
- Transparency Diligence means being honest about AI's role in our work with everyone who needs to know
- Deployment Diligence requires taking responsibility for verifying and vouching for the outputs we use or share
- Different contexts (personal, academic, professional) may have different expectations for disclosure and verification
- Thoughtful Diligence helps ensure our AI collaborations are not only effective and efficient, but also ethical and safe

# Claude 101

- Before your next conversation with Claude, consider: setting the stage (your role, objectives, and context), defining the task (what action you want Claude to take), and specifying rules (style, tone, and examples).
- Memory automatically saves key context from your conversations — your role, preferences, past decisions, and working style — so you don't have to repeat yourself every time you start a new chat. For example, if you tell Claude you work in marketing at a B2B company, it'll remember that context going forward.
- Styles let you customize how Claude communicates. Choose from preset options — like concise, formal, or explanatory — or create your own custom style by describing exactly how you want Claude to write.
- Projects are self-contained workspaces with their own memory, chat histories, knowledge bases, and customized instructions. Think of them as dedicated environments for specific work streams.
  - Project instructions guide Claude's behavior—you can specify tone, expertise level, response style, and more. These instructions apply to every conversation within the project.
  - Projects scale automatically. When your knowledge base approaches context limits, Claude switches to searching your project knowledge and pulling in only what's relevant, expanding capacity by up to 10x while maintaining response quality.
  - For Claude for Work users, projects enable collaboration. Share projects with teammates so everyone benefits from the same context, instructions, and accumulated knowledge.

## Artifacts

- Artifacts are standalone, interactive outputs that Claude creates in a dedicated window alongside your conversation. Instead of getting a long block of code or text buried in the chat, you see your content rendered and ready to use—whether that's a working website, an interactive chart, or a document you can immediately download.
  - It's significant and self-contained, typically over 15 lines
  - It's something you're likely to want to edit, iterate on, or reuse
  - It represents complex content that stands on its own without needing the surrounding conversation
  - It's content you'll want to reference or use later
  - We can ask Claude to create an artifact.
  - Artifacts have their own dedicated window.
  - Artifacts can be shared.

## Connectors

- Connectors transform Claude from an assistant into an informed collaborator by giving Claude access to the same tools, data, and context that you use every day. Instead of starting every conversation from scratch, Claude can work directly with your actual information.
- Connectors allow Claude to read information and perform actions on your behalf. Depending on the connector and permissions you grant, Claude can search your files, retrieve documents, analyze data, create new content, update records, and execute tasks across your connected applications—all from within your conversation.
- The Model Context Protocol (MCP) powers connectors. Think of MCP like USB-C for AI—a universal standard that allows Claude to connect to many different applications through a single, consistent interface. This open standard means developers can build connectors for any tool, and those connectors work seamlessly with Claude.
- There are two types of connectors: web connectors and desktop extensions. Web connectors link Claude to cloud services like Google Drive, Notion, Slack, and Asana. Desktop extensions run locally on your computer through the Claude Desktop app, giving Claude access to local files and native applications.

### Enterprise search

- Enterprise Search adds a dedicated "Ask {Your Org Name}" option to your sidebar. This is designed specifically for finding and synthesizing knowledge buried across your company's tools and data sources.
- When you ask a question, Claude searches across all your connected tools—such as SharePoint documents, Slack conversations, Gmail threads, and Google Drive files—and synthesizes information into a unified response. Plus, it always cites its sources so you can get the full context.
- Enterprise Search requires a two-step setup process: first an admin configures it for the organization, then individual users authenticate with their personal accounts.

### Research

- Research transforms how Claude finds and analyzes information. Instead of a single search, Claude operates agentically—conducting multiple searches that build on each other while determining exactly what to investigate next. It explores different angles of your question automatically and works through open questions systematically.
- Research takes longer than your usual search — a few minutes or more, depending on the question.
- Research works with Thinking, so Claude can plan its approach before it searches.
- It's designed for situations where a thorough understanding requires pulling together information from multiple sources, comparing different perspectives, and synthesizing findings into actionable insights.
- Claude autonomously decides what to search next based on what it has already found, pursuing leads and filling gaps without you needing to direct each step.

# Claude Code 101

- The context window. Think of this as Claude's working memory. It can hold a lot, but not everything at once. This is where the "agentic" aspect comes in — Claude finds strategic ways to locate answers within your codebase without loading the entire thing into context.
- It asks for permission. By default, Claude Code will ask you before running commands or making changes.
- Tools are the backbone of how agents work. Most AI assistants simply take text in and return text out. Tools let Claude Code determine when to execute code to get closer to completing a task. This could be a file-reading tool, a web search tool, or any number of other capabilities.
- Claude Code has several permission modes:
  - Default behavior: Claude asks for explicit permission before editing a file or running a shell command.
  - Auto-accept: Files are edited without asking, but commands still require approval.
  - Plan mode: Uses read-only tools to compile a plan of action before starting any work.
- Explore, Plan, Code, and Commit
- Code
  - Define a success criteria. For Claude to be confident in its results, it needs to be clear on what "correct" looks like. Make this explicit when writing your plan.
  - Add tools. Tools that help Claude complete its goals remove a lot of back and forth. For example, if you're building web UIs, install the Claude in Chrome extension so Claude Code can control a browser tab and test the UI directly.
  - Include a test suite. Give Claude a test suite it can continuously validate against. Claude can even write tests for you. Before handing this off, make sure the tests are a reliable source of truth to avoid false positives.

## Context

- When you approach the limit, the context window is automatically compacted. Compaction summarizes important details and removes unnecessary tool call results to free up space. Note that this process can potentially lose details.
- If you want to completely start from scratch with no memory of the previous session, run /clear. This removes everything.
- To check the state of your context, run the /context command. You'll get a high-level overview of your context size, the categories taking up the most space, and a visual graphic showing the breakdown.
- Tips
- Be specific. A vague prompt might seem smaller, but it actually costs more context in the long run.
- Manage your MCP servers. MCP servers load all of their available tools into context by default, even when you're not using them. If you have servers configured for things unrelated to the current project, consider turning them off. You can also try "Skills," which work similarly to MCP servers but don't load everything into context upfront.
- Use subagents. Subagents run in parallel with your main agent but have a completely separate context window. For tasks where you only need the answer — like "where are the authentication endpoints located?" — a subagent does the work and returns just a summary to your main agent, keeping your primary context clean.

## Code review

- The /commit-push-pr skill handles the commit, push, and PR creation all in one step. Instead of doing each manually, just run the skill and Claude takes care of it.
- When Claude creates a PR through gh pr create, the session gets linked to that PR automatically. If you need to come back to it later — maybe to address review comments or fix a failing build — run:
  `claude --from-pr <PR_NUMBER>`
  This picks up right where you left off.

## CLAUDE.md

- Types
  - Project-level CLAUDE.md lives in the root directory of your project. Shared with the team.
  - User-level CLAUDE.md lives in your configuration folder. This one is just for you and applies across all your projects. Put your personal preferences here.
- Save corrections to memory. If you find yourself correcting Claude repeatedly — like telling it to always use server actions instead of API routes — explicitly ask Claude to save that rule to memory. Next time you open the project, it'll know.
- Reference project docs. If you have documentation in your project that you want Claude to reference, use the @ symbol with the file path.

## Subagents

- Claude spawns a subagent to handle a task like "explore this codebase for me." The subagent runs in parallel with its own context window, does all the exploration work, and once finished, summarizes its findings and returns that summary back to Claude.
- Subagents are defined in Markdown files with YAML frontmatter. The easiest way to get started is to let Claude generate one for you. Run: `/agents`
- Customization
  - Persistent memory lets your subagent retain memory across conversations. This is great if you're using it consistently on the same projects.
  - Preload skills into subagents by adding the skill key and listing skills by name. Note that unlike skills in your main conversation, the entire skill is loaded into context here.

## Tools

- You can add MCP servers with the claude mcp add command.
- Types
  - HTTP servers are for remote services. These are hosted by the service provider and connect over the network.
  - Stdio servers are for local processes that run on your machine.
- You can manage your servers with /mcp inside a Claude Code session to see what's connected, check status, and disable servers you don't need.
- Scopes
  - Local — only available in the current project, just for you.
  - User — available across all your projects.
  - Project — uses a .mcp.json file that you check into version control so anyone on the codebase gets the exact same servers automatically.
- MCP servers add tool definitions to your context window — even when you're not actively using them. If you have a lot of servers configured, this eats into your available context. If a tool has a CLI equivalent (like gh for GitHub or aws for AWS), the CLI is more context-efficient because it doesn't add persistent tool definitions. You might also benefit from using a Skill instead.
  - A Skill has a name and description loaded into context, and Claude only loads the full skill contents when it determines it needs to use it.
  - If your MCP tools exceed 10% of your context window, Claude Code automatically switches to tool search mode, which discovers the right tools on demand — though this may not work as reliably.

## Hooks

- Hooks let you run commands at specific points in Claude Code's lifecycle. The key difference between hooks and everything else covered in this course is that hooks are deterministic — they always run.
- Hooks are configured in your settings.json. You pick an event, optionally set a matcher for which tools it applies to, and provide a command to run. The available events are:
  - PreToolUse — runs before a tool call
  - PostToolUse — runs after a tool call completes
  - UserPromptSubmit — runs when you submit a prompt, before Claude processes it
  - Stop — runs when Claude finishes responding
  - Notification — runs when Claude sends a notification
- You configure them through the /hooks command inside Claude Code, or by editing settings.json directly.
- The most common hook: auto-formatting after edits. Set a PostToolUse hook with a matcher of "Edit|MultiEdit|Write" so it fires whenever Claude modifies a file. The command checks the file extension and runs the appropriate formatter — Prettier for TypeScript, gofmt for Go, whatever your project uses.
- PreToolUse hooks can block tool calls before they execute. Your hook receives the tool name and input as JSON on stdin. The exit code determines the behavior:
  - Exit code 0 — proceed normally.
  - Exit code 2 — block the action. The stderr message gets fed back to Claude as feedback so it knows why it was blocked and can adjust.
  - Any other exit code — a non-blocking error that gets shown to you but doesn't stop anything.
- Hooks configured in .claude/settings.json are project-level and can be checked into your repo. This means your entire team gets the same hooks automatically. Use the CLAUDE_PROJECT_DIR environment variable in your commands to reference scripts stored in your project, so they work regardless of Claude's current working directory.

# Claude Code in Action

## Long sessions

- `/compact` might lose data so give instructions about how to compact
- When Claude heads down the wrong path, you don't have to prompt your way back out. Rewind takes you to your last checkpoint. Every user prompt creates a checkpoint you can revert to. To open the menu, double tap escape on an empty prompt.
  - Restore code and conversation - roll back both together.
  - Restore conversation - roll back just the chat.
  - Restore code - roll back just the files.
  - Summarize from here - summarizes everything after the checkpoint. Great if you had a side conversation and just want to free up some space.
  - Summarize up to here - summarizes everything before the checkpoint. Great when you had a long setup phase you want to compress, but you want to keep the implementation parts intact.
- `/goal`: Goal sets a completion condition. You describe what "done" looks like, and Claude keeps working across turns until a fast evaluator confirms those conditions are met. It won't just stop the first time it thinks it's finished.
  - To cancel it, run `/goal clear`. One important constraint: the evaluator only reads the transcript. So your condition has to be checkable from the output Claude actually produces, like the results of a test run.
- Loop runs a prompt on an interval between turns, either fixed or self-paced. Use it to pull something external, like a CI run or a deploy, and act when the state changes. To stop a loop, just press escape.
- Parallel work with worktrees: There's one helpful file to know about. A .worktreeinclude file at the repo root lists git-ignored files to copy into each worktree. This is useful for things like an environment variable file or a local config that you need in every worktree but don't want to commit to version control.

## CLAUDE.md

- The leaner the file, the more of it Claude actually follows.
- scopes
  - Managed policy — the org-level file your platform team controls. You can't exclude it, so org policy is always in play.
  - User — your personal preferences that follow you across every project on your machine.
  - Project — the file shared with your team, checked into the repo.
  - Local — ignored by git. Your personal notes for this one repository only.
- When your project file starts getting long, you can break it into pieces using the path-to-file import syntax. Instead of one wall of text, you point to other files: `@.claude/conventions/code-style.md`
- Be specific.
- When you tell Claude not to do something, say what to do instead.
- Words like "IMPORTANT" and "YOU MUST" do raise a rule's priority. But only relative to everything quieter around it.

## Verification skills

- When it finishes, the change matches the skill's description, so the skill fires on its own. From there it:
  - Runs the test suite.
  - Reads the diff.
  - Checks that no test was weakened just to make things pass.
  - Reports pass or fail, with the evidence attached.
- skill folder extras:
  - Drop a reference.md next to the skill for detailed material, then link to it from skill.md. Claude only reads it when it actually needs that depth. Your main file stays short.
  - Put scripts in the folder too. Claude executes them rather than loading their contents into context. That means a skill can carry its own tooling, like a check.sh that runs all the gates.

## Permission modes

- Types:
  - Manual reads only, without prompting. Everything else asks first.
  - Accept edits runs reads, file edits, and common file system bash commands without asking. This is for iterating on code that you review after the fact.
  - Plan reads only. It researches and proposes changes without editing anything.
  - Auto accepts everything, with a separate classifier model reviewing each action before it runs.
  - Don't ask allows only pre-approved tools. Everything else is auto-denied with no prompt.
    - Don't ask is the right move whenever no human is around to approve prompts: CI pipelines, scheduled jobs, overnight batches. Only pre-approved tools are allowed, and anything off that list gets auto-denied with no prompt.
  - Bypass permissions skips all checks. This is the equivalent of the dangerously-skip-permissions flag. Only run it inside an isolated container or virtual machine.
- Auto mode classifier prohibits e.g.
  - Production deploys and migrations
  - Force pushing, or piping downloaded code straight into a shell
  - Sending sensitive data to external endpoints
  - Destroying files that exist for the session

## Hooks

- Claude Code fires around 30 hook events over the course of a session.
- The ones worth knowing:
  - PreToolUse fires before a tool call. This is your enforcement primitive. It's the one that can stop something before it happens.
  - PostToolUse fires after a successful tool call. This is usually where auto-formatting or an auto-lint goes.
  - Stop fires when Claude wants to end its turn. You can refuse and say "no, you're not done yet" if some condition isn't met. There's a matching SubagentStop for when a sub-agent finishes.
  - PreCompact and PostCompact fire before and after compaction.
  - InstructionsLoaded fires when a CLAUDE.md or rule file loads. Handy for auditing what actually made it into context.
  - SessionStart fires at the start and primes the environment. Use the startup source if you only want it on fresh starts.
- One thing that trips people up: to re-inject context after compaction, don't use PostCompact. Use SessionStart with the compact matcher. That's the one that actually gets its output back into the conversation.

### PreToolUse

- PreToolUse is where the real power is, because it can block a tool call before it runs. The way you talk back to Claude is by printing JSON and exiting zero. The key field is permissionDecision, and it takes one of three values:
  - allow — let the call through
  - deny — stop the call
  - ask — hand it back to the user to decide
  - There's technically a fourth value, defer, but it only applies to non-interactive -p runs where a calling process pauses the tool and resumes it later. You'll rarely reach for it.

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "...",
    "updatedInput": {
      "command": "..."
    }
  }
}
```

- Notice updatedInput. Instead of blocking a call, you can rewrite it. That's how you'd redact a secret out of a bash command and still let it run. One catch: updatedInput replaces the whole input object, so you have to echo back the fields you aren't changing, or you'll lose them.

### Other hooks

- Not every hook needs to speak JSON. For simpler hooks, exit codes do the job. There are three numbers that matter.
  - 0 is success. If standard out is JSON, Claude parses it. Plain text is ignored on most events, but on SessionStart, UserPromptSubmit, and UserPromptExpansion, plain text gets added to context. That's exactly what makes a state-preserver hook work.
  - 2 is a blocking error. Standard error gets fed back to Claude as context. This is the blocking exit code almost everywhere.
  - Anything else is non-blocking. Standard error gets logged, and Claude carries on.
- A couple more wrinkles. Exit 2 can even block Stop, which is how you tell Claude it's not done. But PostToolUse fires after the tool already ran, so blocking there is too late to stop the call, though it can still feed text back to Claude. And a few events ignore blocking entirely, like Notification and SessionStart. They'll show your standard error and carry on regardless.
- Let's tie it together with something practical. Say you want a PreToolUse guardrail on the Bash tool. The matcher picks the tool to watch, and an optional if clause can narrow it to a specific command.
  - The obvious move is to return deny and stop a dangerous call. That's good. But the lesser-known and more interesting move is to return updatedInput to rewrite the call. That's how you strip a secret out of a command and still let it run, instead of just refusing.
  - Here's what that looks like in practice. Claude is asked to run a command that includes a live-looking secret. The hook intercepts it, spots the sk*live* pattern, and swaps it for a placeholder before the command ever executes.
- One more pattern worth setting up. When Claude compacts a long conversation, it drops a lot of detail. A SessionStart hook with the compact matcher runs right after compaction. Have it print a short summary of the files you've been working on. That summary goes back into context, so Claude picks up where it left off instead of starting cold.

## Routines and headless

### Routine

- A routine is the most direct way to automate a task. There's no script and no server. It bundles three things: a prompt, the repository it works on, and any connectors it needs. Then it runs that bundle in the cloud whenever it's triggered.
- The key part is that the infrastructure is Anthropic's. There's no machine of yours staying on overnight, and there's no workflow file for you to maintain. You describe the job once and it just runs.
- A routine can fire on a few kinds of triggers:
  - A cron schedule, like every morning at 9am.
  - An HTTP POST to its API endpoint, so your own code can kick it off.
  - A GitHub event, like a new pull request landing.
- How to create:
  - You can create a routine from the web at claude.ai/code/routines. You give it a name, write the instructions describing what Claude should do in each session, pick a repository, and choose a trigger.
  - You can also create one from inside Claude Code without leaving your terminal. Just run the /schedule command and describe what you want in plain language
- Limitations:
- Routines are a research preview. Behavior and limits will keep moving, so don't be surprised if things change.
- A recurring schedule runs at most hourly. If you need something more frequent, routines aren't the tool.
- Each run starts from a fresh clone of your default branch and can only push to claude/ prefixed branches unless you loosen that per repo. This is the guardrail that keeps an autonomous run from rewriting main.

### Headless

- But sometimes the job needs your environment, or logic wrapped around the run. That's when you drop to headless mode.
- The core of headless mode is the -p flag (short for --print). It runs Claude Code as a one-shot command with no interactive UI. It reads standard in and writes standard out, so it pipes like any other shell tool: `claude -p "summarize the changes in this diff"`
- One thing worth knowing: -p skips auto-discovery of hooks, skills, plugins, MCP servers, and the CLAUDE.md file. You get Claude plus the tools you allow explicitly, and nothing the local environment happens to load. The upside is that startup is much faster this way.
- The object that matches your schema lands in the structured_output field of the JSON response. So you can pull it out with a jq command and pipe it into a database or another script: `claude -p "Extract the exported function names from src/core/style.js" \
--output-format json \
--json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}' \
| jq '.structured_output.functions'`
- For work that happens across multiple steps, you don't have to cram everything into one command. Capture the session's ID from the JSON output and resume it later: `claude --resume "$(jq -r .session_id /tmp/plan.json)"`
- When CI needs the same results every single run, there's a mode built for that. The --bare flag gives you deterministic mode. It's the right choice when you're running Claude Code inside a pipeline and you want repeatable, predictable output rather than anything that varies run to run.
- The last step on the spectrum is the Agent SDK. This gets you a library that embeds Claude Code inside your own TypeScript or Python applications.
  - Both languages expose a query function and the same primitives as the CLI. You pass a prompt plus options, like:
    - allowedTools to control what Claude can do,
    - a system prompt,
    - and a permission mode.
  - Then you iterate over the messages Claude streams back and handle them however your app needs. It's the same engine as the CLI, just callable from inside your product.

## GH actions and code review

### Code review

- It's an Anthropic-hosted service that reviews your pull requests through the Claude GitHub app. There's nothing for you to build or host. You turn it on, and it starts posting findings as inline comments right on the lines that matter. An organization admin enables it from the Claude Code admin settings.
- From there the admin installs the Claude GitHub app, picks which repos it watches, and decides when it runs. You have a few choices for timing:
  - Once when a PR opens
  - On every push to the PR
  - Only when someone comments @claude review
- A set of review agents analyzes the diff against your full codebase, not just the changed lines in isolation. Then it posts findings as inline comments on the specific lines, tagged by severity, with a summary table in the check run.
- Limitations:
  - It never approves or blocks the PR. The judgment call stays with a human. Claude flags things; you decide.
  - There's no managed autofix. The service posts findings only.
  - It's a research preview right now, available on team and enterprise plans, so expect the behavior to keep moving.
- From your own terminal, the /code-review command reviews a diff, and its --fix flag applies the findings to your working tree.

### GH action

- This is for custom CI: implementing changes from a comment, running scheduled reports, anything you'd normally write a workflow for. It runs the agent on PR comments, scheduled jobs, and any GitHub event.
- Setup starts inside Claude Code. Run the /install-github-app command. You'll need repo admin to do this. The slash command walks you through installing the GitHub app and setting the Anthropic API key secret on the repo.
- The action itself is anthropics/claude-code-action@v1. Here are the inputs you'll actually use:
  - anthropic_api_key — optional.
  - github_token — defaults to secrets.GITHUB_TOKEN.
  - trigger_phrase — what the action listens for in comments. Defaults to @claude.
  - use_bedrock / use_vertex — switch to those providers if you're on Bedrock or Vertex.
  - prompt — the instruction for the run.
  - claude_args — a string of CLI arguments passed straight through to Claude Code.
- Drop a workflow into .github/workflows/claude.yaml and it listens for @claude on PR comments and issue comments. The core step looks like this:

```yaml
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    github_token: ${{ secrets.GITHUB_TOKEN }}
    trigger_phrase: "@claude"
    prompt: "Your instructions here"
    claude_args: "--max-turns 5 --model claude-sonnet-5"
```

- Now someone writes @claude implement the spec in the linked Linear issue on a pull request, and the action picks it up. Claude pushes commits and posts comments describing what it did.
- The same action works for a daily rollup. A cron trigger fires at, say, 9:00 UTC, the action runs, and Claude posts the results. You can also add a workflow_dispatch trigger so you can kick it off manually from the Actions tab.
- The claude_args line is where the fine-tuning happens. A few knobs worth knowing:
  - --max-turns 5 puts a hard cap on the agent loop, so it can't run forever.
  - Permission mode. For an unattended job you'll want it to not stop and ask, since there's no one there to answer.
  - Allowed tools. Give the job exactly what it needs and nothing more. For a report, that means read-only.

## Verifying unsupervised runs

- When a run goes unattended at work, keep it in auto mode rather than bypass permissions. In auto mode, the classifier still reviews each action for danger. That's a safety net worth keeping. But be clear about what that net does and doesn't do. The classifier never judges whether the code is actually correct. It only flags dangerous actions. So your verification bar stays exactly where it was. Set that bar based on how unsupervised the run was.
- The real gate on an unsupervised run is whether the tests passed, and whether Claude actually ran them or only claimed that it did. Don't leave that to trust. Wire it as a hook so Claude can't skip it.
- A couple of hooks do the job:
  - A stop hook that runs your tests and refuses to end the turn on a failure.
  - A post-tool-use hook that lints and type checks after every edit.
- The key detail is the exit code. A hook that exits with exit 2 feeds the failure straight back to Claude. Claude reads that failure and fixes it without you asking. Best of all, the check fires on every run, whether or not you remember to ask for it.
- The sub-agent code review you'd run before a pull request works here too. Point it at an unsupervised run.

## Plugins

- A plugin is one installable unit. It bundles everything you'd otherwise share by hand: skills, subagents, hooks, and MCP server configs, plus the longer tail of stuff like language server protocol servers, background monitors, themes, and a slice of settings.json. One version, one install.
- Inside a session: `/plugin install org-name@plugin-name`
  - Claude Code installs it and tells you to run /reload-plugins to apply the change.
- For a team, the better move is to add a private marketplace once. A marketplace is a shared source that plugins resolve through: `/plugin marketplace add your-org/claude-plugins`
- Once it's added, every install after that resolves through it. You get centralized discovery, version tracking, and updates in one place instead of scattered across everyone's laptop. You can browse what's available from the Discover tab.
- Here's the part that matters most. A plugin runs code on your machine, with your privileges. Its hooks fire on every matching tool call. So if you install a plugin for its skills, you also get its PreToolUse and Stop hooks whether you read them or not.
- Before you install, check the plugin's details. Claude Code shows you what it will install and estimates the context cost, along with a plain warning that Anthropic doesn't control what's inside third-party plugins.
- A plugin doesn't overwrite your configuration. Its components run alongside your own. e.g. A plugin's PreToolUse hook and your own PreToolUse hook both fire on every tool call.
- Skills, agents, and commands are namespaced under the plugin name, so they never clash with yours. A plugin can also ship a settings.json file, but only a narrow one. Claude Code honors just two keys from it: the agent and subagent status line keys.
  - That agent key is worth a pause. Setting it promotes one of the plugin's subagents to the main thread, along with its system prompt, tool restrictions, and model. In other words, enabling the plugin can change how Claude Code behaves by default.
- Once a plugin is installed you can see everything it added, manage it, and uninstall it from the plugin panel.

### Packaging a plugin

- A plugin uses the same .claude shape you already use:
  - One folder per skill.
  - One markdown file per subagent under agents.
  - hooks/hooks.json and .mcp.json, at the plugin root.
- On top of that, there's an optional manifest. It lives at .claude-plugin/plugin.json and holds the name, version, description, and author:

```js
{
  "name": "svg-splitter-review",
  "version": "0.1.0",
  "description": "Reviews the SVG Splitter repo",
  "author": {
    "name": "Lewis Menelaws"
  }
}
```

- The manifest is optional. Leave it out and Claude Code still discovers your components by directory convention. But a couple of details are worth knowing:
  - Name is the only required field. It namespaces your skills as company-name:skill-name, which keeps them from colliding with anyone else's.
  - Version it like any other dependency. That's what makes updates and version tracking work across your team.

# Introduction to agent skills

- This video introduces skills — reusable markdown files that teach Claude Code how to handle specific tasks automatically. Instead of repeating instructions every time you ask Claude to review a PR or write a commit message, you write a skill once and Claude applies it whenever the task comes up.
- Each skill lives in a SKILL.md file with a name and description in its frontmatter
- Claude uses the description to match skills to requests.
- Personal skills go in ~/.claude/skills and follow you across all projects. Project skills go in .claude/skills inside a repository
  - On Windows, personal skills live in C:/Users/<your-user>/.claude/skills
- Skills load on demand — unlike CLAUDE.md (which loads into every conversation) or slash commands (which require explicit invocation), skills activate automatically when Claude recognizes the situation
- When Claude matches a skill to your request, you'll see it load in the terminal
- Claude loads only skill names and descriptions at startup
- You get a confirmation prompt before Claude loads the full skill content into context
- Priority for name conflicts: Enterprise → Personal → Project → Plugins
- Always restart Claude Code for changes to take effect when updating or deleting a skill
- You can verify it's available by checking the available skills list.

## Advanced skills

- The agent skills open standard
- name and description are required — allowed-tools and model are optional but powerful additions
  - If you omit allowed-tools entirely, the skill doesn't restrict anything.
- A good description answers two questions: What does the skill do? When should Claude use it?
- keep SKILL.md under 500 lines and link to supporting files (references, scripts, assets) that Claude reads only when needed
- Scripts execute without loading their contents into context — only the output consumes tokens, keeping context efficient
- Skills share Claude's context window with your conversation. When Claude activates a skill, it loads the contents of that SKILL.md into context.
  - scripts/ — Executable code
  - references/ — Additional documentation
  - assets/ — Images, templates, or other data files
- The script executes and only the output consumes tokens. The key instruction to include in your SKILL.md is to tell Claude to run the script, not read it.
- Sharing
  - Project skills in .claude/skills
  - Plugins
  - Enterprise managed settings deploy skills organization-wide with the highest priority
  - Subagents don't automatically see your skills — you must explicitly list skills in a custom agent's frontmatter skills field Skills are loaded when the subagent starts, not on demand like in the main conversation.
  - Built-in agents (Explorer, Plan, Verify) can't access skills at all — only custom subagents defined in .claude/agents can

## Skills vs others

- Use Subagents when:
  - You want to delegate a task to a separate execution context
  - You need different tool access than the main conversation
  - You want isolation between delegated work and your main context

## Troubleshooting

- Start with the skills validator tool. using uv is the easiest way to get it set up quickly
- If a skill doesn't trigger, the cause is almost always the description
- If a skill doesn't load, check that SKILL.md is inside a named directory (not at the skills root) and the file name is exactly SKILL.md. claude --debug
- For runtime errors, check dependencies, file permissions (chmod +x), and path separators (use forward slashes everywhere)
- Installed a plugin but can't see its skills? Clear the cache, restart Claude Code, and reinstall. If skills still don't appear after that, the plugin structure might be wrong.

# Introductio to MCP

- Think of it as a way to shift the burden of tool definitions and execution away from your server to specialized MCP servers.
- Each MCP Server acts as an interface to some outside service.
- Anyone can create an MCP server implementation. Often, service providers themselves will make their own official MCP implementations.
- MCP servers and tool use are complementary but different concepts. MCP servers provide tool schemas and functions already defined for you, while tool use is about how Claude actually calls those tools.

## MCP client

- The MCP client serves as the communication bridge between your server and MCP servers. It's your access point to all the tools that an MCP server provides, handling the message exchange and protocol details so your application doesn't have to.
- The most common setup runs both the MCP client and server on the same machine, communicating through standard input/output.
- ListToolsRequest/ListToolsResult: The client asks the server "what tools do you provide?" and gets back a list of available tools.
- CallToolRequest/CallToolResult: The client asks the server to run a specific tool with given arguments, then receives the results.

### Steps

1. User Query: The user submits their question to your server
2. Tool Discovery: Your server needs to know what tools are available to send to Claude
3. List Tools Exchange: Your server asks the MCP client for available tools
4. MCP Communication: The MCP client sends a ListToolsRequest to the MCP server and receives a ListToolsResult
5. Claude Request: Your server sends the user's query plus the available tools to Claude
6. Tool Use Decision: Claude decides it needs to call a tool to answer the question
7. Tool Execution Request: Your server asks the MCP client to run the tool Claude specified
8. External API Call: The MCP client sends a CallToolRequest to the MCP server, which makes the actual GitHub API call
9. Results Flow Back: GitHub responds with repository data, which flows back through the MCP server as a CallToolResult
10. Tool Result to Claude: Your server sends the tool results back to Claude
11. Final Response: Claude formulates a final answer using the repository data
12. User Gets Answer: Your server delivers Claude's response back to the user

![Event loop](/assets/claude2mcpclient.png)

## Defining tools with MCP

- use the official Python SDK

```python
# server start
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("DocumentMCP", log_level="ERROR")
```

```python
# tool definition
@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string."
)
def read_document(
    doc_id: str = Field(description="Id of the document to read")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")

    return docs[doc_id]
```

## The MCP server inspector

- When building MCP servers, you need a way to test your functionality without connecting to a full application. The Python MCP SDK includes a built-in browser-based inspector that lets you debug and test your server in real-time.

```bash
# starts the inspector with GUI
mcp dev mcp_server.py
```

- The inspector maintains your server state between tool calls, so edits persist and you can verify the complete functionality of your MCP server.

## Implementing an MCP client

- The client is what allows our application code to communicate with the MCP server and access its functionality.
- In most real-world projects, you'll either implement an MCP client or an MCP server
- 2 components:
  - MCP Client - A custom class we create to make using the session easier
  - Client Session - The actual connection to the server (part of the MCP Python SDK). The client session requires careful resource management - we need to properly clean up connections when we're done. That's why we wrap it in our own class that handles all the cleanup automatically.

### Client functions

- two essential functions: list_tools() and call_tool()

```python
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools
```

- It's straightforward - we access our session (the connection to the server), call the built-in list_tools() method, and return the tools from the result.

```python
async def call_tool(
    self, tool_name: str, tool_input: dict
) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)
```

- We pass the tool name and input parameters (provided by Claude) to the server and return the result.

## Resources

- Resources in MCP servers allow you to expose data to clients, similar to GET request handlers in a typical HTTP server. They're perfect for scenarios where you need to fetch information rather than perform actions.
- Let's say you want to build a document mention feature where users can type @document_name to reference files. This requires two operations:
  - Getting a list of all available documents (for autocomplete)
  - Fetching the contents of a specific document (when mentioned)
- Resources follow a request-response pattern. When your client needs data, it sends a ReadResourceRequest with a URI to identify which resource it wants. The MCP server processes this request and returns the data in a ReadResourceResult.

### Resource types

- Direct resources have static URIs that never change. They're perfect for operations that don't need parameters.

```python
@mcp.resource(
    "docs://documents",
    mime_type="application/json"
)
def list_docs() -> list[str]:
    return list(docs.keys())
```

- Templated resources include parameters in their URIs. The Python SDK automatically parses these parameters and passes them as keyword arguments to your function.

```python
@mcp.resource(
    "docs://documents/{doc_id}",
    mime_type="text/plain"
)
def fetch_doc(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    return docs[doc_id]
```

- Use the mime_type parameter to give clients a hint about what kind of data you're returning:
  - "application/json" for structured data
  - "text/plain" for plain text
  - "application/pdf" for binary files
- You can test resources using the MCP Inspector.

### Accessing resources

- To enable resource access in your MCP client, you need to implement a read_resource function.
- When you request a resource, the server returns a result with a contents list. We access the first element since we typically only need one resource at a time. The response includes:
  - The actual content (text or data)
  - A MIME type that tells us how to parse the content
  - Other metadata about the resource

## Prompts

- Prompts in MCP servers let you define pre-built, high-quality instructions that clients can use instead of writing their own prompts from scratch. Think of them as carefully crafted templates that give better results than what users might come up with on their own.
- Prompts work best when they're specialized for your MCP server's domain.

```python
@mcp.prompt(
    name="format",
    description="Rewrites the contents of the document in Markdown format."
)
def format_document(
    doc_id: str = Field(description="Id of the document to format")
) -> list[base.Message]:
    prompt = f"""
Your goal is to reformat a document to be written with markdown syntax.

The id of the document you need to reformat is:
<document_id>
{doc_id}
</document_id>

Add in headers, bullet points, tables, etc as necessary. Feel free to add in structure.
Use the 'edit_document' tool to edit the document. After the document has been reformatted...
"""

    return [
        base.UserMessage(prompt)
    ]
```

- The list_prompts method is straightforward. It calls the session's list prompts function and returns the prompts

```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts
```

- The get_prompt method is more interesting because it handles variable interpolation. For example, if your server has a format_document prompt that expects a doc_id parameter, the arguments dictionary would contain {"doc_id": "plan.md"}. This value gets interpolated into the prompt template.

```python
async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages
```

# Advanced MCP

## Sampling

- Sampling allows a server to access a language model like Claude through a connected MCP client. Instead of the server directly calling Claude, it asks the client to make the call on its behalf. This shifts the responsibility and cost of text generation from the server to the client.
- Sampling is most valuable when building publicly accessible MCP servers. You don't want random users generating unlimited text at your expense. By using sampling, each client pays for their own AI usage while still benefiting from your server's functionality.
- The flow is straightforward:
  - Server completes its work (like fetching Wikipedia articles)
  - Server creates a prompt asking for text generation
  - Server sends a sampling request to the client
  - Client calls Claude with the provided prompt
  - Client returns the generated text to the server
  - Server uses the generated text in its response
- The client integrates with the language model, handles API keys and cost

#### Setting up sampling

- In your tool function on the server, use the create_message function to request text generation:

```python
@mcp.tool()
async def summarize(text_to_summarize: str, ctx: Context):
    prompt = f"""
    Please summarize the following text:
    {text_to_summarize}
    """

    result = await ctx.session.create_message(
        messages=[
            SamplingMessage(
                role="user",
                content=TextContent(
                    type="text",
                    text=prompt
                )
            )
        ],
        max_tokens=4000,
        system_prompt="You are a helpful research assistant",
    )

    if result.content.type == "text":
        return result.content.text
    else:
        raise ValueError("Sampling failed")
```

- Create a sampling callback on the client that handles the server's requests:

```python
async def sampling_callback(
    context: RequestContext, params: CreateMessageRequestParams
):
    # Call Claude using the Anthropic SDK
    text = await chat(params.messages)

    return CreateMessageResult(
        role="assistant",
        model=model,
        content=TextContent(type="text", text=text),
    )
```

- Then pass this callback when initializing your client session:

```python
async with ClientSession(
    read,
    write,
    sampling_callback=sampling_callback
) as session:
    await session.initialize()
```

## Logging and progress notifications

-In the Python MCP SDK, logging and progress notifications work through the Context argument that's automatically provided to your tool functions. This context object gives you methods to communicate back to the client during execution.

- context.info() - Send log messages to the client
- context.report_progress() - Update progress with current and total values

```python
@mcp.tool(
    name="research",
    description="Research a given topic"
)
async def research(
    topic: str = Field(description="Topic to research"),
    *,
    context: Context
):
    await context.info("About to do research...")
    await context.report_progress(20, 100)
    sources = await do_research(topic)

    await context.info("Writing report...")
    await context.report_progress(70, 100)
    results = await generate_report(sources)

    return results
```

- On the client side, you need to set up callback functions to handle these notifications. The server emits these messages, but it's up to your client application to decide how to present them to users.
- You provide the logging callback when creating the client session, and the progress callback when making individual tool calls. This gives you flexibility to handle different types of notifications appropriately.

```python
async def logging_callback(params: LoggingMessageNotificationParams):
    print(params.data)

async def print_progress_callback(
    progress: float, total: float | None, message: str | None
):
    if total is not None:
        percentage = (progress / total) * 100
        print(f"Progress: {progress}/{total} ({percentage:.1f}%)")
    else:
        print(f"Progress: {progress}")

async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(
            read,
            write,
            logging_callback=logging_callback
        ) as session:
            await session.initialize()

            await session.call_tool(
                name="add",
                arguments={"a": 1, "b": 3},
                progress_callback=print_progress_callback,
            )
```

## Roots

- Roots are a way to grant MCP servers access to specific files and folders on your local machine. Think of them as a permission system that says "Hey, MCP server, you can access these files" - but they do much more than just grant permission.
- Roots also provide security by limiting access to specific folder.
- Flow
  - User asks to convert a video file
  - Claude calls list_roots to see what directories it can access
  - Claude calls read_dir on accessible directories to find the file
  - Once found, Claude calls the conversion tool with the full path
- The MCP SDK doesn't automatically enforce root restrictions - you need to implement this yourself. A typical pattern is to create a helper function like is_path_allowed() that:
  - Takes a requested file path
  - Gets the list of approved roots
  - Checks if the requested path falls within one of those roots
  - Returns true/false for access permission
  - You then call this function in any tool that accesses files or directories before performing the actual file operation.

## JSON message types

- MCP uses JSON messages to handle communication between clients and servers.
- Here's a typical example: when Claude needs to call a tool provided by an MCP server, the client sends a "Call Tool Request" message. The server processes this request, runs the tool, and responds with a "Call Tool Result" message containing the output.
- The complete list of message types is defined in the official MCP specification repository on GitHub.
- 2 types of messages:
  - request-result
  - notification

### Request-result messages

- These messages always come in pairs. You send a request and expect to get a result back:
  - Call Tool Request → Call Tool Result
  - List Prompts Request → List Prompts Result
  - Read Resource Request → Read Resource Result
  - Initialize Request → Initialize Resu

### Notification messages

- These are one-way messages that inform about events but don't require a response:
  - Progress Notification - Updates on long-running operations
  - Logging Message Notification - System log messages
  - Tool List Changed Notification - When available tools change
  - Resource Updated Notification - When resources are modified

### Client and Server messages

- Client messages include requests that clients send to servers (like tool calls) and notifications that clients might send.
- Server messages include requests that servers send to clients and notifications that servers broadcast.
- Some transports, like the streamable HTTP transport, have limitations on which types of messages can flow in which directions.

## STDIO transport

- The client launches the MCP server as a subprocess and communicates through standard input and output streams.
- Here's how it works:
  - Client sends messages to the server using the server's stdin
  - Server responds by writing to stdout
  - Either the server or client can send a message at any time
  - Only works when client and server run on the same machine
- Every MCP connection must start with a 3 step handshake:
  - Initialize Request - Client sends this first
  - Initialize Result - Server responds with capabilities
  - Initialized Notification - Client confirms (no response expected)
  - Only after this handshake can you send other requests like tool calls or prompt listings.
- 4 types
  - Client → Server request: Client writes to stdin
  - Server → Client response: Server writes to stdout
  - Server → Client request: Server writes to stdout
  - Client → Server response: Client writes to stdin

## Streamable HTTP transport

- The streamable HTTP transport enables MCP clients to connect to remotely hosted servers over HTTP connections. Unlike the standard I/O transport that requires both client and server on the same machine, this transport opens up possibilities for public MCP servers that anyone can access.
- Two key settings control how the streamable HTTP transport behaves:
  - stateless_http - Controls connection state management
  - json_response - Controls response format handling
  - By default, both settings are false, but certain deployment scenarios may force you to set them to true. When enabled, these settings can break core functionality like progress notifications, logging, and server-initiated requests.
- he following message types become difficult to implement with plain HTTP:
  - Server-initiated requests: Create Message requests, List Roots requests
  - Notifications: Progress notifications, Logging notifications, Initialized notifications, Cancelled notifications

### The details

- StreamableHTTP is MCP's solution to a fundamental problem: some MCP functionality requires the server to make requests to the client, but HTTP makes this challenging.
- Uses Server-Sent Events (SSE)
- The process starts like any MCP connection:
  - Client sends an Initialize Request to the server
  - Server responds with an Initialize Result that includes a special mcp-session-id header
  - Client sends an Initialized Notification with the session ID
- After initialization, the client can make a GET request to establish a Server-Sent Events connection. This creates a long-lived HTTP response that the server can use to stream messages back to the client at any time.
- When the client makes a tool call, things get more complex. The system creates two separate SSE connections:
  - Primary SSE Connection: Used for server-initiated requests and stays open indefinitely
  - Tool-Specific SSE Connection: Created for each tool call and closes automatically when the tool result is sent
  - Progress notifications: Sent through the primary SSE connection
  - Logging messages and tool results: Sent through the tool-specific SSE connection

### The state

- In case I have many MCP Server instances behind a load balancer, stateful connections won't work as the traffic won't be routed back to the same Server every time.
- The json_response=True flag is simpler - it just disables streaming for POST request responses. Instead of getting multiple SSE messages as a tool executes, you get only the final result as plain JSON.
- With streaming disabled:
  - No intermediate progress messages
  - No log statements during execution
  - Just the final tool result

# Building with the Cladude API

## Accessing Claude with the API

- When your server contacts the Anthropic API, you can use either an official SDK or make plain HTTP requests.
- Must have params:
  - API Key - Identifies your request to Anthropic
  - Model - Name of the model to use (like "claude-3-sonnet")
  - Messages - List containing the user's input text
  - Max Tokens - Limit for how many tokens Claude can generate
- 4 steps how Claude processes the request:
  - Tokenization: Claude first breaks your input text into smaller chunks called tokens. These can be whole words, parts of words, spaces, or symbols. For simplicity, think of each word as one token.
  - Embedding: Each token gets converted into an embedding - a long list of numbers that represents all possible meanings of that word. Think of embeddings as numerical definitions that capture semantic relationships.
  - Contextualization: Claude refines each embedding based on surrounding words to determine the most likely meaning in context. This process adjusts the numerical representations to highlight the appropriate definition.
  - Generation: The contextualized embeddings pass through an output layer that calculates probabilities for each possible next word. Claude doesn't always pick the highest probability word - it uses a mix of probability and controlled randomness to create natural, varied responses.
- Conditions on when Claude stops generating the response:
  - Max tokens reached - Has it hit the limit you specified?
  - Natural ending - Did it generate an end-of-sequence token?
  - Stop sequence - Did it encounter a predefined stop phrase?
- The API response structure:
  - Message - The generated text
  - Usage - Count of input and output tokens
  - Stop Reason - Why generation ended

## Making a request

- Params:
  - model - The name of the Claude model you want to use
  - max_tokens - A safety limit on response length (not a target)
  - messages - The conversation history you're sending to Claude

```python
from anthropic import Anthropic

client = Anthropic()
model = "claude-sonnet-4-0"

message = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=[
        {
            "role": "user",
            "content": "What is quantum computing? Answer in one sentence"
        }
    ]
)
```

- Message types: Each message is a dictionary with a role (either "user" or "assistant") and content (the actual text).
  - User messages - Content you want to send to Claude (written by humans)
  - Assistant messages - Responses that Claude has generated
- The response: `message.content[0].text`

## Multi turn conversation

- Claude doesn't store any of your conversation history, each request you make is completely independent, with no memory of previous exchanges.
- Here's the flow that actually works:
  - Send your initial user message to Claude
  - Take Claude's response and add it to your message list as an assistant message
  - Add your follow-up question as another user message
  - Send the entire conversation history to Claude

## System prompts

- System prompts provide Claude with guidance on how to respond. You define them as plain strings and pass them into the create function call.
  - System prompts provide Claude guidance on how to respond
  - Claude will try to respond in the same way someone in the specified role would respond
  - Helps keep Claude on task

```python
system_prompt = """
You are a patient math tutor.
Do not directly answer a student's questions.
Guide them to a solution step by step.
"""

client.messages.create(
    model=model,
    messages=messages,
    max_tokens=1000,
    system=system_prompt
)
```

## Temperature

- Temperature is a powerful parameter that controls how predictable or creative Claude's responses will be.
- 3 keys steps when Claude figures out the next word:
  - Tokenization - Breaking your input into smaller chunks
  - Prediction - Calculating probabilities for possible next words
  - Sampling - Choosing a token based on those probabilities
- At low temperatures (near 0), Claude becomes very deterministic - it almost always picks the highest probability token. At high temperatures (near 1), Claude distributes probability more evenly across options, leading to more varied and creative outputs.

```python
 params = {
        "model": model,
        "max_tokens": 1000,
        "messages": messages,
        "temperature": temperature
    }
```

## Response streaming

- Instead of waiting for the full response from Claude, we can stream its response.
- When you enable streaming, Claude sends back several types of events:
  - MessageStart - A new message is being sent
  - ContentBlockStart - Start of a new block containing text, tool use, or other content
  - ContentBlockDelta - Chunks of the actual generated text
  - ContentBlockStop - The current content block has been completed
  - MessageDelta - The current message is complete
  - MessageStop - End of information about the current message

```python
stream = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=messages,
    stream=True
)

for event in stream:
    print(event)
```

- Rather than manually parsing events, you can use the SDK's simplified streaming interface that extracts just the text content:

```python
with client.messages.stream(
    model=model,
    max_tokens=1000,
    messages=messages
) as stream:
    for text in stream.text_stream:
        print(text, end="")

     # Get the complete message for database storage
    final_message = stream.get_final_message()
```

## Structured data

- When you need Claude to generate structured data like JSON, Python code, or bulleted lists, you'll often run into a common problem: Claude wants to be helpful and add explanatory text around your content.
- You can combine assistant message prefilling with stop sequences to get exactly the content you want.
- This technique works by:
  - The user message tells Claude what to generate
  - The prefilled assistant message makes Claude think it already started a markdown code block
  - Claude continues by writing just the JSON content
  - When Claude tries to close the code block with ```, the stop sequence immediately ends generation

````python
messages = []

add_user_message(messages, "Generate a very short event bridge rule as json")
add_assistant_message(messages, "```json") # starts the JSON instead of Claude

text = chat(messages, stop_sequences=["```"]) # when Claude ends the JSON it also signals it to stop generating

import json

# Clean up and parse the JSON
clean_json = json.loads(text.strip())
````

## Prompt evaluation

- Prompt engineering gives you techniques for writing better prompts, while prompt evaluation helps you measure how well those prompts actually work.
- Prompt engineering is your toolkit for crafting effective prompts. It includes techniques like:
  - Multishot prompting
  - Structuring with XML tags
  - Many other best practices
- Prompt evaluation takes a different approach. Instead of focusing on how to write prompts, it's about measuring their effectiveness through automated testing. You can:
  - Test against expected answers
  - Compare different versions of the same prompt
  - Review outputs for errors
- 5 steps to eval a prompt
  - Start by writing an initial prompt that you want to improve.
  - Your evaluation dataset contains sample inputs that represent the types of questions or requests your prompt will handle in production. The dataset should include questions that will be interpolated into your prompt template.
  - Take each question from your dataset and merge it with your prompt template to create complete prompts. Then send each one to Claude to get responses.
  - The grader evaluates the quality of Claude's responses by examining both the original question and Claude's answer. This step provides objective scoring, typically on a scale from 1 to 10, where 10 represents a perfect answer and lower scores indicate room for improvement.
  - Now that you have a baseline score, you can modify your prompt and run the entire process again to see if your changes improve performance.
- There are three main approaches to grading model outputs:
  - Code graders - Programmatically evaluate outputs using custom logic
  - Model graders - Use another AI model to assess the quality
  - Human graders - Have people manually review and score outputs
- Before implementing any grader, you need clear evaluation criteria. For a code generation prompt, you might focus on:
  - Format - Should return only Python, JSON, or Regex without explanation
  - Valid Syntax - Produced code should have valid syntax
  - Task Following - Response should directly address the user's task with accurate code
- For a grader, the key insight is asking for strengths, weaknesses, and reasoning alongside the score. Without this context, models tend to default to middling scores around 6.

## Prompt engineering

- The evaluation setup uses a PromptEvaluator class that handles dataset generation and model grading. When creating your evaluator instance, you can control concurrency with the max_concurrent_tasks parameter. Start with a low concurrency value (like 3) to avoid rate limit errors.

```python
evaluator = PromptEvaluator(max_concurrent_tasks=5)
```

- The evaluation system can automatically generate test cases based on your prompt requirements. You define what inputs your prompt needs:

```python
dataset = evaluator.generate_dataset(
    task_description="Write a compact, concise 1 day meal plan for a single athlete",
    prompt_inputs_spec={
        "height": "Athlete's height in cm",
        "weight": "Athlete's weight in kg",
        "goal": "Goal of the athlete",
        "restrictions": "Dietary restrictions of the athlete"
    },
    output_file="dataset.json",
    num_cases=3
)
```

- Start with a simple, naive prompt to establish a baseline:

```python
def run_prompt(prompt_inputs):
    prompt = f"""
What should this person eat?

- Height: {prompt_inputs["height"]}
- Weight: {prompt_inputs["weight"]}
- Goal: {prompt_inputs["goal"]}
- Dietary restrictions: {prompt_inputs["restrictions"]}
"""

    messages = []
    add_user_message(messages, prompt)
    return chat(messages)
```

- When running your evaluation, you can specify additional criteria that the grading model should consider:

```python
results = evaluator.run_evaluation(
    run_prompt_function=run_prompt,
    dataset_file="dataset.json",
    extra_criteria="""
The output should include:
- Daily caloric total
- Macronutrient breakdown
- Meals with exact foods, portions, and timing
"""
)
```

- After running an evaluation, you'll get both a numerical score and a detailed HTML report. The report shows you exactly how each test case performed, including the model's reasoning for each score.

### Prompt engineering techniques

- The first line of your prompt is the most important part of your entire request. This is where you set the stage for everything that follows, and getting it right can dramatically improve your results.
- Focus on two key principles: clarity and directness.
- Details:
  - Use instructions, not questions
  - Start with direct action verbs like "Write," "Create," or "Generate"
- Output quality guidelines: listing qualities that your output should have.
- Process steps: Provides specific steps for Claude to follow. This approach is particularly useful when you want Claude to think through a problem systematically or consider multiple perspectives before arriving at a final answer.
- XML tags: Claude can sometimes struggle to understand which pieces of text belong together or what different sections are supposed to represent. XML tags provide a simple way to add structure and clarity to your prompts. e.g. `<sales_records>`...`</sales_records>`
- One-shot (single example) or multi-shot (multiple examples) prompting: giving Claude sample input/output pairs to guide its responses.
  - Don't just provide the input/output pair - explain why the output is good.
