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

## API

### Chat

- Opus > Sonnet > Haiku
- API request details
  - API key
  - Model: model to use
  - Messages: list of messages containing the user's input
  - Max Tokens: limit how many tokens can be generated max
- steps of processing user input
  - break message into tokens e.g. 1 word = 1 token
  - each token is converted to an embedding (a long list of numbers) ~ the number definition of that word
    - a word can mean a lot of things based on its context -> the embeddings represent all those meanings -> to find the correct meaning of the word in the user input we have to contextualize each word: each embedding is adjusted based on the other embeddings around it
  - generation: the output layer generates a possibility of next words from the embeddings -> but not the most probable ones are selected, it depends on many factors which ones are selected
  - the model asks itself if the generation is done
    - e.g. check max token
  - the response has:
    - the text
    - usage. count number of input and output tokens
    - stop reason: why the model decided to stop generating
  - we receive this on our backend and we can return to the client what we want from these
- client.messages.create(model, max_tokens, messages=[])
  - a message = {role: "user", content="What is a dog?"}
  - response: many properties but the answer is in the text property
- multi turn conversation: the API does not store any messages! we always have to send all the former messages

#### System prompts
- customize the style and tone of the response
- client.messages.create(model, messages, max_tokens, system)
  - system prompt e.g. "You are a tutor who ... You should not give the answer directly at first ..."


#### Temperature
- defines how predictable or creative the responses are
- the text generation process
  1. tokenization: breaking the input into chunks
  2. prediction? calculating probabilities for the next possible words
  3. sampling: chosing a token based on the probabilities
- the cat is ... cute (0.3), hungry (0.2), brown(0.1) ...
  - these are the probabilities of the next word
  - LOW temperature will nearly always choose the most probable (cute, 0.3) -> deterministic
  - HIGH temperature distributes the probability more evenly -> more creative as more chances to pick different words each time
- it's just a config param:
```python
 params = {
        "model": model,
        "max_tokens": 1000,
        "messages": messages,
        "temperature": temperature
    }
```

#### Response streaming
- answers can take 10-30 seconds and we don't want the user to just stare at the screen for that long -> let's stream the response continously
- types of response messages:
  - MessageStart - A new message is being sent
  - ContentBlockStart - Start of a new block containing text, tool use, or other content
  - ContentBlockDelta - Chunks of the actual generated text: THIS is what we display to the user
  - ContentBlockStop - The current content block has been completed
  - MessageDelta - The current message is complete
  - MessageStop - End of information about the current message
- handle streaming:
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

- use the SDK to extract only the texts:
```python
with client.messages.stream(
    model=model,
    max_tokens=1000,
    messages=messages
) as stream:
    for text in stream.text_stream:
        print(text, end="")
```

- get the FULL message after streaming:
```python
with client.messages.stream(
    model=model,
    max_tokens=1000,
    messages=messages
) as stream:
    for text in stream.text_stream:
        # Send each chunk to your client
        pass
    
    # Get the complete message for database storage
    final_message = stream.get_final_message()
```

#### Structured data
- when we want structured data e.g. JSON and don't want any noise
-  add assistant message prefilling + stop sequences
```python
messages = []

add_user_message(messages, "Generate a very short event bridge rule as json")
# it sets role: assistant in the parameter and prints the ```json as if Claude added it
add_assistant_message(messages, "```json")

# stop_sequences is a parameter on the req object
text = chat(messages, stop_sequences=["```"])
```
  - the prefilled assistant message makes Cluade think it already started a markdown code block (START)
  - when it tries to close the code block with ```, the stop sequence ends the generation (END)

### Prompt evaluation
- to test the correctness of a prompt
- 5 steps eval flow
  1. draft a prompt
  2. create an eval dataset: sample inputs e.g. what kind of question or requests will Claude receive
  3. feed through Claude: merge my eval dataset with me draft prompt and send each one to Claude
  4. feed through a Grader: scores Claude's answers to the questions from 1 to 10
    - it can create an average score too which we can compare to the next runs
  5. change prompt and repeat

#### Generating data sets
- the eval dataset can be a list of objects with a `task` property that has the user input

