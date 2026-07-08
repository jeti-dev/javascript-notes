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

#### Model based grading
- grader types: code, another AI model, human
- some grading tips: format, syntax, task following
- when creating a model Grader it is very important to ask for strengths, weaknesses and reasoning otherwise it will default to score 6

### Prompt engineering
#### Clear and direct
- the first line is crucial, use clear and direct language
  - use instructions not questions
  - start with direct action verbs like write, create, generate
- 3 questions for you to write a better starting instruction: 
  - What action to take? 
  - What to create? 
  - What are the key constraints?
- Claude works the best when we treat it like a capable assistant whi needs clear direction

#### Specific
- output quality guidelines: length of response, structure and format, specific elements to include, style requirements
- process steps (step by step instructions that affect Claude's thinking): troubleshooting complex problems, decision making scenarios, critical thinking, when considering multiple angles
  - e.g. consider recent organization changes + review customer feedback + identify relevant industry changes

#### Structure with XML
- when there is a big context + serves as delimeters

#### Provide examples
- e.g. is a sentence has a positive or negative sentiment
- goal: capturing edge cases, defining complex output format, showing exact style, how to handle ambiguous inputs
- one-shot: provide 1 example
- multi shot: multiple examples to show different types of responses or edge cases
- tip: use the highest scoring answers from the evaluations as the examples
- some explanation after the example makes it even better, it helps Claude understand the reasoning

- summary
  - be clear and direct
  - be specific
  - structure with xml
  - provide examples


### Tools
- a way for Claude to get extra data e.g. if it has no access to the internet, Claude doesn't know what's the weather at the moment
- how tool use works
  1. ask a question from Claude including instructions on how to get extra data
  2. Claude asks for extra data with details on what is needed
  3. we respond with that extra data to Claude
  4. Claude gives the final response

#### Tool functions
- a function that gets called by Claude
  - should have a meaningful name, parameter names, validate inputs, raise menaningful errors -> Claude will try to call it a second time on error
- e.g.
```python 
def get_current_datetime(date_format="%Y-%m-%d %H:%M:%S"):
    if not date_format:
        raise ValueError("date_format cannot be empty")
    return datetime.now().strftime(date_format)
```
#### Tool schemas
- a JSON schema providing meta info about the tool function
- properties:
  - name
  - description
  - input_schema
- e.g.
```json
{
    "name": "get_current_datetime",
    "description": "Returns the current date and time formatted according to the specified format",
    "input_schema": {
        "type": "object",
        "properties": {
            "date_format": {
                "type": "string",
                "description": "A string specifying the format of the returned datetime. Uses Python's strftime format codes.",
                "default": "%Y-%m-%d %H:%M:%S"
            }
        },
        "required": []
    }
}
```
- how to:
  - 3-4 sentences to explain what it does
  - describe when to use it
  - explain the kind of data it returns
  - add detailed descriptions for the arguments
- use the ToolParam type from the Anthropic lib for better type checking

#### Message blocks
- to enable tool use, set it in the config:
```python
response = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=messages,
    tools=[get_current_datetime_schema],
)
```
- when Claude wants to use a tool, it returns a message with multiple blocks
  - text block: human readable text explaining what Claude is doing
  - tool use block: instructions for my code about which tool to call and what parameters to use; props:
    1. ID for tracking the tool call
    2. name of the function to call
    3. input params as a dictionary
    4. the type "tool_use"
- remember that we always have to send all the previous messages to Claude -> we have to send the tool use block too:
```python
messages.append({
    "role": "assistant",
    "content": response.content
})
```

#### Sending tool results
- unpack the parameters from Claude's tool use request to be used in my tool function:
```python
get_current_datetime(**response.content[1].input)
```
- return the tool result to Claude in a special format:
  - tool_use_id: the id from the tool request
  - content: output from my tool, formatted as a string
  - is_error: only true if an error occured
```python
{
        "type": "tool_result",
        "tool_use_id": response.content[1].id,
        "content": "15:04:22",
        "is_error": False
}
```
- claude can request multiple tool use in one message
- when sending the follow up request with the tool result, we must still include the tool schema so Claude can understand the tool references in the history!

#### Multi turn conversations
- when Claude asks for multiple tool use one after the other to generate the answer -> we need a conversation loop that continues until Claude stops requesting tools
```python
def run_conversation(messages):
    while True:
        response = chat(messages)

        add_assistant_message(messages, response)

        # Pseudo code
        if response isn't asking for a tool:
            break

        tool_result_blocks = run_tools(response)
        add_user_message(messages, tool_result_blocks)
        
    return messages
```
- how to know when Claude doesn't need more tools:
```python
if response.stop_reason != "tool_use":
    break  # Claude is done, no more tools needed
```
- handling tool error:
```python
try:
    tool_output = run_tool(tool_request.name, tool_request.input)
    tool_result_block = {
        "type": "tool_result",
        "tool_use_id": tool_request.id,
        "content": json.dumps(tool_output),
        "is_error": False
    }
except Exception as e:
    tool_result_block = {
        "type": "tool_result", 
        "tool_use_id": tool_request.id,
        "content": f"Error: {e}",
        "is_error": True
    }
```

#### Tools and streaming
- for tool use, a new type of event called InputJsonEvent is returned by Clade; props:
  - partial_json: a chunk of JSON representing part of the tool arguments
  - snapshot: the cumulative JSON built up from all the chunks received so far
- how to process it:
```python
for chunk in stream:
    if chunk.type == "input_json":
        # Process the partial JSON chunk
        print(chunk.partial_json)
        # Or use the complete snapshot so far
        current_args = chunk.snapshot
```
- Claude does not return all the chunks immediately as they get generated, it buffers and validates them first e.g.
```python
{
  "abstract": "This paper presents a novel...",
  "meta": {
    "word_count": 847,
    "review": "This paper introduces QuanNet..."
  }
}
```
  - It starts to generate chunks for the "abstract" property and its value. When it sees that it reached the end of the value, it validates if this key value pair is a valid text in the schema (a valid parameter for my tool) I sent. If it is then Claude returns the buffered chunks at once.
- if we don't want buffering + validation so we can get the chunks faster, then we can use fine grained tool calling -> add fine_grained=True to the API call
  - if this is enabled, then we have to handle invalid JSON responses from Claude

#### The built-in text edit tool