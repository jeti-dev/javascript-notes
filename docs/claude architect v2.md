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

Claude Code 101

- The context window. Think of this as Claude's working memory. It can hold a lot, but not everything at once. This is where the "agentic" aspect comes in — Claude finds strategic ways to locate answers within your codebase without loading the entire thing into context.
- It asks for permission. By default, Claude Code will ask you before running commands or making changes.
