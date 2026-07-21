---
title: Claude Architect
layout: default
---

# Claude Architect

From [https://anthropic.skilljar.com/](https://anthropic.skilljar.com/)

## Claude Code in action

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
-  add assistant message prefilling (e.g. start a JSON markdon block) + stop sequences
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
- features
  - view file or directories
  - view ranges of lines in a file
  - replace text in a file
  - create new file
  - insert text at specific lines
  - undo recent edits
- Claude has the schema but I have to provide the implementation functions
- I also still have to provide a small schema for Claude so it can expand it to the full schema
```python 
def get_text_edit_schema(model):
    if model.startswith("claude-3-7-sonnet"):
        return {
            "type": "text_editor_20250124",
            "name": "str_replace_editor",
        }
    elif model.startswith("claude-3-5-sonnet"):
        return {
            "type": "text_editor_20241022", 
            "name": "str_replace_editor",
        }
```

#### Web search tool
- it must be enabled by my organization
- Claude has the implementation function and the schema too but I still have to provide a simple schema to enable it
```python
web_search_schema = {
    "type": "web_search_20250305",
    "name": "web_search", 
    "max_uses": 5
}
```
- returned blocks
  - Text blocks - Claude's explanation of what it's doing
  - ServerToolUseBlock - Shows the exact search query Claude used
  - WebSearchToolResultBlock - Contains the search results
  - WebSearchResultBlock - Individual search results with titles and URLs
  - Citation blocks - Text that supports Claude's statements
- restrict domains with `allowed_domains`:
```python
web_search_schema = {
    "type": "web_search_20250305",
    "name": "web_search",
    "max_uses": 5,
    "allowed_domains": ["nih.gov"]
}
```
- just enable it in the tools and Claude will decide when to use it

### RAG and agentic search
#### Intro to RAG
- retrievel augmented generation
- helps to work with large documents which are too big to fit into single propmpts -> breaks the document into chunks and only includes the most relevant pieces when answering questions

#### Chunking strategies
1. size based
  - divide into equal length strings
  - includes some overlap on each side
  - might break up related conntent into separate chunks
2. structure based
  - divide by headers, paragraphs etc
  - try to avoid breaking related content into multiple chunks
  - we must first understand the document to chunk it effectively
3. semantic based
  - divide by related sentences or sections
  - we must understand the meaning of all sentences
  - expensive to do but results in more relevant chunks
4. sentence based
  - a practical middle ground
  - split into individual sentences using regex then group them into chunks with some overlap

#### Text embeddings
- after creating the chunks now we have to search the relevant chunks somehow
- a text embedding is a numerical representation of the meaning contained in some text
- the process
  - we feed the text into an embedding model
  - we get a long list of numbers ranging from -1 to +1
- we can't know what these numbers represent because the meanings are just learnt by the models during training and they are not interpretable by humans
- a provider for embeddings is e.g. VoyageAI

#### The RAG flow
- the embeddings are stored in a vector database
- at this point, we are waiting for a user input
- when an input arrives, we run it through the same embedding model
- we get embeddings for the input
- we send it to the vector db to get the most relevant chunks
  - it uses cosine similarity , the value is between -1 to 1
  - -1 is very different, 0 is no relationship, 1 is high similarity
  - cosine distance: 1 - cosine similarity where values close 0 mean high similarity while larger values mean less similarity
- when we have the most relevant chunk, we create a prompt and send it to Claude

#### BM25 lexical search
- semantic search doesn't always return the best results => sometimes we need exact term matches
- BM25 (best match 25, an algorithm for lexical search in RAG systems) = combine semantic search + lexical search: run them parallel and merge the results
- e.g.
  - we want so search for an incident number "a inc-123"
  - it gets tokenized to ['a', 'inc-123']
  - we count how many times each token appears in all of the chunks
  - the less times a token is present, the bigger importance it gets e.g. "a" appears a lot more time than "inc-123"
  - we'll only use those chunks which have the most HIGH scoring tokens

#### Multi index RAG pipeline
- combine the semantic search (vector embeddings) + lexical search (BM25) => use a technique called reciprocal rank fusion
- the difficulty: we have 2 different ways to search chunks and we somehow have to merge them

| Chunk name | Rank from vector | Rank from BM25 |
|------------|------------------|----------------|
| Topic 2    | 1                | 2              |
| Topic 6    | 2                | 3              |
| Topic 5    | 3                | 1              |

```python
RRF_score(d) = Σ(1 / (k + rank_i(d)))

# Where k is a constant (usually 60) and rank_i(d) 
# is the rank of document d in the i-th ranking
# e.g. 1.0/(60+2) + 1.0/(60+3) for Topic 6
```
- it combines the scoring of both searching methods and the highest numbers win

### Features of Claude

#### Extended thinking
- not compatible with some features e.g. message pre-filling and temperature
- reasoning feature that gives the model time to work through complex problems - we can see the reasoning process too
- we get a new block: ThinkingBlock e.g. "I should cover what recursion is, how it works ..."
  - type: thinking
  - thinking: What Claude is saying (what we display to the user)
  - signature: a cryptographic token tied to the thinking text Claude generated => it prevent developers from tampering with the reasoning process
- more expensive + takes more time
- when to use? => use prompt evaluation to decide if extended thinking is needed
- redacted thinking: when the thinking process gets flagged by internal safety systems 
  - the redacted content is in encrypted form and can be passed back to Claude without losing context
  - by sending a special trigger warning to Claude we can test is our app handles this case gracefully
- two parameters to enable it
  - thinking=True
  - thinking_budget=1024 (it's the minimum, must be smaller than the max_tokens)

#### Image support
- describe what is in the image, compare images, count objects, complex visual analysis
- limits
  - max 100 images across all messages in a single request
  - max 5mb per image
  - when sending 1 image then max height/width of 8000px
  - hwn sending multiple images then max height/width of 2000px
  - can be bas64 or a URL too
  - token calculation for an image: (width px * height px) / 750
- we have to use an ImageBlock
```python
with open("image.png", "rb") as f:
    image_bytes = base64.standard_b64encode(f.read()).decode("utf-8")

add_user_message(messages, [
    # Image Block
    {
        "type": "image",
        "source": {
            "type": "base64",
            "media_type": "image/png",
            "data": image_bytes,
        }
    },
    # Text Block
    {
        "type": "text",
        "text": "What do you see in this image?"
    }
])
```
- use the same prompt enginnering techniques e.g.
  - add detailed guidelines and analysis steps
  - use one or multi shot examples
  - break down into smaller steps
  - e.g. identify one ball at a time and assign a number to them => then count the numbers from left to right, from the bottom to the top => how many balls are in the image?

#### PDF support
- what Claude can do
  - understand text content
  - understand images and charts
  - understand tables and data relationships
  - understand document structure and formatting
```python
with open("earth.pdf", "rb") as f:
    file_bytes = base64.standard_b64encode(f.read()).decode("utf-8")

messages = []

add_user_message(
    messages,
    [
        {
            "type": "document",
            "source": {
                "type": "base64",
                "media_type": "application/pdf",
                "data": file_bytes,
            },
        },
        {"type": "text", "text": "Summarize the document in one sentence"},
    ],
)
```

#### Citations
- Claude can reference parts of the text we provide to prove that its response is not from its training data but from the document
```python
{
    "type": "document",
    "source": {
        "type": "base64",
        "media_type": "application/pdf",
        "data": file_bytes,
    },
    "title": "earth.pdf", # required
    "citations": { "enabled": True } # required
}
```
- the response becomes more complex:

| Citation Structure | Example | Purpose |
| :--- | :--- | :--- |
| **cited_text** | Earth's atmosphere and oceans were formed by volcanic activity and outgassing. | Text that Claude is citing from the provided document |
| **document_index** | 0 | If multiple documents are provided, this will tell you which document Claude is citing from |
| **document_title** | 'earth.pdf' | Title of the document that Claude is citing from |
| **start_page_number** | 4 | Starting page of the cited text |
| **end_page_number** | 5 | Ending page of the cited text |

- citations with plain text sources:
```python
{
    "type": "document", 
    "source": {
        "type": "text",
        "media_type": "text/plain",
        "data": article_text,
    },
    "title": "earth_article",
    "citations": { "enabled": True }
}
```

#### Prompt caching
- faster response + reduced cost
- in a normal flow Claude:
  - tokenizes the input prompt
  - creates embedding for each token
  - adds context based on surrounding text
  - only then generates the output
  - these are all thrown away after the response is sent
- e.g. as a first step we summarize a text but then we ask for more focus on something => Claude could reuse some of its previous work
  - tokenization, ebeddings and added context can be saved to a cache
  - cache lives for 1 hour
  - only useful if we repeadetly sendinf the same content e.g. multiple questions about a document
- how to set it up
  - we have to add a 'cache breakpoint' to a block
  - work done before the breakpoint will be cached
  - cache will be used only if the content up to and including the breakpoint is the same
```python
user_message = {
  "role": "user",
  "content": [
    {
      "type": "text",
      "text": "<Long prompt>"
      "cache_control": {
       "type": "ephemeral"
      }
    }
  ]
}
```
- IMPORTANT: the cache is dismissed if anything has changed in the previously cached input
  - e.g. one of your prompts has a dynamic data e.g. time(). Because AI is stateless, you always have to send all the previous conversations. So for the second time if you time() runs again, it would return a new time in one of your *older* inputs and it would dismiss the cache
- can be added to
  - text blocks
  - system prompts
  - tool definitions
  - image blocks
  - tool use and tool result blocks
- tool definitions and system prompts are most commonly cached as they rarely change between requests
- a request is processed in a specific order: tools, system prompt, messages => it helps thinking about where to put cache breakpoint
- up to 4 cache breakpoint can be added
- content must be min 1024 tokens to be cached - it's the sum of all messages and blocks, not individual blocks
```python
# tool schema caching
last_tool["cache_control"] = {"type": "ephemeral"}

# system prompts caching
params["system"] = [
        {
            "type": "text",
            "text": system,
            "cache_control": {"type": "ephemeral"}
        }
    ]
```
- some new patterns in the response
  - first request: cache_creation_input_otkens=1772-
  - follow up requests: cache_read_input_tokens=1772-
  - changed content: new cache creation tokens appear

#### Code execution and the file API
- file API: we can use this instead of encoding images or PDF directly in the message => we can upload files ahead of time and reference them later
- steps
  - upload file to Claude in a separate API call
  - receive a file metadata object containing a unique file ID
  - reference that file ID in future messages
- code execution tool: server based tool, I don't have to provide the implementation
  - Python code in an isolated Docker container
  - no network access
  - Claude can execute code multiple times during a single conversation
  - results are captured and interpreted by Claude for the final response
- combining them
  - upload the data file with the File API
  - then reference it and run analytics code on the data
```python
messages = []
add_user_message(
    messages,
    [
        {
            "type": "text",
            "text": """Run a detailed analysis to determine major drivers of churn.
            Your final output should include at least one detailed plot summarizing your findings."""
        },
        {"type": "container_upload", "file_id": file_metadata.id},
    ],
)

chat(
    messages,
    tools=[{"type": "code_execution_20250522", "name": "code_execution"}]
)
```
- returned blocks:
  - TextBlocks: Claude's analyzis and explanations
  - ServerToolUseBlocks: the actual code that was run
  - CodeExecutionRoolResultBlocks: output from running the code
- downloading a file: look for the code_execution_output block which contains a file ID for the generated content (e.g. charts)

### MCP
- provides tools, prompts and resources for Claude

#### MCP clients 
- the bridge between my server and the MCP servers, it handles the message passing nd protocol details
- the MCP server usually runs on our server
- it supports multiple transport protocols: HTtp, websocket, other
- new message types
  - from client to server: ListToolsRequest, CallToolRequest
  - from server to client: ListToolsResult, CallToolResult

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant OurServer as Our Server
    participant MCPClient as MCP Client
    participant MCPServer as MCP Server
    participant Claude
    participant Github

    User->>OurServer: What repositories do I have?
    OurServer->>MCPClient: I need a list of tools to send to Claude
    MCPClient->>MCPServer: ListToolsRequest
    MCPServer-->>MCPClient: ListToolsResult
    MCPClient-->>OurServer: Here are the tools
    OurServer->>Claude: Query + Tools
    Claude->>OurServer: ToolUse
    OurServer->>MCPClient: Please run this tool with these args
    MCPClient->>MCPServer: CallToolRequest
    MCPServer->>Github: Request to Github
    Github-->>MCPServer: Response
    MCPServer-->>MCPClient: CallToolResult
    MCPClient-->>OurServer: Here's the result of running the tool
    OurServer->>Claude: toolResult
    Claude->>OurServer: Your repositories are...
    OurServer->>User: Your repositories are...
  ```
- there is a Python SDK that helps with creating tools
  - it has a built in browser based tool (server inspector) testing app

#### MCP client
- allows our app to communicate with the MCP server
- in the real world we usually build either an MCP server or an MCP client
- main components:
  - MCP Client: a custom class that helps using the session
  - Client Session: the acutal connection to the server (part of the MCP SDK)
    - must be cleaned up after usage
- two main purposes:
  - lets our code get a list of tools to be passed to Claude
  - lets our code call a tool when Claude requests them
- two methods to implement:
  - `list_tools()`
  - `call_tool()`

```python
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools
  
async def call_tool(
    self, tool_name: str, tool_input: dict
) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)
  
# test it by running the client directly; a testing harness connects to our MCP server and calls the methods
async with MCPClient(
    command="uv", args=["run", "mcp_server.py"]
) as client:
    result = await client.list_tools()
    print(result)
```

#### Defining resources
- expose data to clients (like an HTTP get request); used when we have to fetch info rather than perform actions
- e.g. when the user types @ then he can select a document name from a list of documents
  - we need the MCP server to return a list of document names
  - and we need the MCP server to give us the document content when the message mentions a document
- the client has to send a ReadResourceRequest message with a URL and the MCP server responds with the data
- two types of resources:
  - direct resource: the URI does not have any params
  - templated resource: the URI has some parameters; the SDK parses them and passes to my function

```python
# direct
@mcp.resource(
    "docs://documents",
    mime_type="application/json"
)
def list_docs() -> list[str]:
    return list(docs.keys())

# templated
@mcp.resource(
    "docs://documents/{doc_id}",
    mime_type="text/plain"
)
def fetch_doc(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    return docs[doc_id]
```
- we also return `mime_type` to give clients a hint
- test the resources with the MCP Inspector: `uv run mcp dev mcp_server.py` - it will spin up the UI based testing tool

#### Accessing resources
- we need a `read_resource` function in the MCP client
  - the response from the MCP server contains a `contents` list: the first element is the actual resouce data along with meta data like MIME type
```python
async def read_resource(self, uri: str) -> Any:
    result = await self.session().read_resource(AnyUrl(uri))
    resource = result.contents[0]

# handling MIME time by the MCP client
import json
from pydantic import AnyUrl

if isinstance(resource, types.TextResourceContents):
    if resource.mimeType == "application/json":
        return json.loads(resource.text)
    
    return resource.texts
```

#### Defining prompts
- prompts define a set of user and assistant messages that the users can directly use
- steps to implement
  - use `@mcp.prompt()`
  - add a name and a description
  - return a list of messages that form the complete prompt
  - these prompts should be high quality

```python
# a document formatting prompt
from mcp.server.fastmcp import base

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

{doc_id}


Add in headers, bullet points, tables, etc as necessary. Feel free to add in extra formatting.
Use the 'edit_document' tool to edit the document. After the document has been reformatted...
"""
    
    return [
        base.UserMessage(prompt)
    ]
```
- test it with the UI testing tool

#### Prompts in the client
- implement `list_prompts`
```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts
```

- when we define a prompt function, it can accept paramteres
```python
def format_document(doc_id: str):
    # The doc_id gets interpolated into the prompt
```

- implement getting a prompt with the arguments interpolated, `get_prompt`
```python
async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages
```

- test them through the CLI

### Anthropic apps
#### Claude Code
- `/init` to understand the codebase -> creates a summary in the CLAUDE.md file
- CLAUDE.md places and their scopes
  - project: shared within the project
  - local: personal notes that are not checked into git
  - user: across my projects
- use # to write instructions to the CLAUDE.md file
- steps to use it successfully
  - feed context (only relevant files)
  - tell Claude to just create a plan, don't code yet
  - ask for implementation
  - OR as a test driven development
    - in step 2, let Claude create tests
    - in step 3, implement the tests
    - in step 4, write the code that passes the test
- has a built in MCP client
```shell
claude mcp add [server-name] [command-to-start-server]
```

### Agents and workflows
- workflow: a series of calls to Claude meant to solve a specific problem using predefined steps
- agents: Claude is given a goal and a set of tools => Claude needs to figure out how to solve the problem

#### Workflows
- evaluator-optimizer pattern
  - producer: takes an input and creates an output
  - grader: evaluates the output against some criteria
  - feedback loop: if the grader does't accept the output, feedback goes back to the producer for improvement
  - iteration: the cycle repeats until the grader accept the output
- paralellization e.g. analyze if an object in an image is wood, metal or stone
  - if we add this as 1 big prompt with details about how to recognize all these materials then the result won't be that precise
  - send 3 messages simultaneously and ask for only 1 material => feed the 3 responses to a new session which can return the real answer
- chaining workflows: split up the work into focused steps that build on each other so Claude can focus on small, specific tasks one at a time
- routing workflow: one generic prompt is often not good for different topics e.g. creating an educational vs an entertaining video
  - first, make Claude categorize the input
  - then ask claude to create content and provide the category