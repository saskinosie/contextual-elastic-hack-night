# Build Your Own Agent

Go beyond the Quick Start — design and build a custom AI agent with your own data, prompts, and workflows.

**Work solo or in teams of up to 3 people.**

---

## What You'll Build

Create a custom RAG agent that:
- Uses your own documents or a datastore you configure
- Has specialized prompts tailored to a specific use case
- (Optional) Uses Agent Composer for advanced workflows

---

## Step 1: Choose Your Use Case

Pick a use case that interests you. Here are some ideas:

| Use Case | Description |
|----------|-------------|
| **Technical Documentation Bot** | Answer questions about a codebase, API, or product docs |
| **Research Assistant** | Summarize and synthesize information from research papers |
| **Customer Support Agent** | Handle FAQs and troubleshooting from a knowledge base |
| **Legal/Compliance Helper** | Navigate policy documents and regulations |
| **Content Curator** | Find and recommend relevant content from a library |

---

## Step 2: Create Your Datastore

### Via GUI

1. Go to **Datastores** in the sidebar
2. Click **Create Datastore**
3. Name your datastore and add a description
4. Upload your documents (PDF, TXT, DOCX supported)
5. Wait for ingestion to complete

### Via API

```python
from contextual import ContextualAI

client = ContextualAI(api_key="YOUR_API_KEY")

# Create datastore
datastore = client.datastores.create(name="My Custom Datastore")

# Upload documents
with open("document.pdf", "rb") as f:
    result = client.datastores.documents.ingest(
        datastore_id=datastore.id,
        file=f
    )
    print(f"Uploaded: {result.id}")
```

> **Tip:** For best results, use clean PDFs with selectable text.

---

## Step 3: Design Your Prompts

Great prompts make great agents. Consider these three layers:

### Identity Prompt
*Who is your agent?*

```
You are [ROLE] specializing in [DOMAIN].

Your personality:
- [TRAIT 1]
- [TRAIT 2]
- [TRAIT 3]

You always [BEHAVIOR] and never [ANTI-PATTERN].
```

### Research Guidelines
*When and how should your agent search?*

```
Search the knowledge base when:
- [TRIGGER 1]
- [TRIGGER 2]

Prioritize:
- [PRIORITY 1]
- [PRIORITY 2]

Avoid searching for:
- [EXCEPTION]
```

### Response Guidelines
*How should your agent format answers?*

```
Structure your responses as follows:
1. [SECTION 1]: Brief summary
2. [SECTION 2]: Detailed explanation
3. [SECTION 3]: Sources and citations

Format rules:
- Use [FORMAT STYLE] for [CONTENT TYPE]
- Keep responses [LENGTH CONSTRAINT]
```

---

## Step 4: Create Your Agent

You have enterprise access, which unlocks additional configuration options!

1. Go to **Agents** in the sidebar
2. Click **Create Agent**
3. Fill in **Agent Name** and **Description**
4. Under **Configuration**, choose your approach:

| Option | Best For |
|--------|----------|
| **Template** | Quick start with pre-built templates like Agentic Search, Simple Search, etc. |
| **Prompt** (Beta) | Describe what you want in natural language — AI generates the config |
| **Blank Canvas** (Enterprise) | Full control with Agent Composer — build custom workflows |

5. Under **Datastores**, link the datastore you created in Step 2
6. Click **Create and Customize** to open Agent Composer, or **Start Chatting** to test right away

### Recommended Paths

**Path A: Start with a Template, Then Customize**
1. Select **Template** and choose a pre-built option like **Agentic Search**
2. Link your datastore
3. Click **Create and Customize** to open Agent Composer
4. Edit the prompts and settings to match your use case

**Path B: Use Prompt to Generate a Config (Beta)**
1. Select **Prompt**
2. Describe your agent in natural language (e.g., "A legal assistant that answers questions about contracts")
3. Review and refine the generated configuration

**Path C: Build from Scratch with Agent Composer**
1. Select **Blank Canvas**
2. Design your workflow graph from the ground up
3. Full control over every step and prompt

---

## Step 5: Build with Agent Composer (Optional)

Agent Composer lets you create multi-step workflows using YAML. Here's a starter template:

```yaml
version: 0.1
inputs:
  query: str

outputs:
  response: str

ui_output: response

nodes:
  create_message_history:
    type: CreateMessageHistoryStep
    input_mapping:
      query: __inputs__#query

  research:
    type: AgenticResearchStep
    ui_stream_types:
      retrievals: true
    config:
      tools_config:
        - name: search_docs
          description: |
            Search the document collection for relevant information.
            Use this when the user asks questions that require
            knowledge from the indexed documents.
          step_config:
            type: SearchUnstructuredDataStep
            config:
              top_k: 30
              reranker: "ctxl-rerank-v2-instruct-multilingual-FP8"
              rerank_top_k: 10

      agent_config:
        agent_loop:
          num_turns: 5
          model_name_or_path: "vertex_ai/claude-sonnet-4-5@20250929"

          identity_guidelines_prompt: |
            # Add your identity prompt here

          research_guidelines_prompt: |
            # Add your research guidelines here

    input_mapping:
      message_history: create_message_history#message_history

  generate:
    type: GenerateFromResearchStep
    ui_stream_types:
      generation: true
    config:
      model_name_or_path: "vertex_ai/claude-sonnet-4-5@20250929"

      identity_guidelines_prompt: |
        # Add your identity prompt here

      response_guidelines_prompt: |
        # Add your response guidelines here

    input_mapping:
      message_history: create_message_history#message_history
      research: research#research

  __outputs__:
    type: output
    input_mapping:
      response: generate#response
```

For more details, see the [Agent Composer YAML Guide](https://docs.contextual.ai/how-to-guides/agent-composer-yaml).

---

## Step 6: Test and Iterate

Test your agent with various queries:

1. **Happy path** — Questions your agent should handle well
2. **Edge cases** — Unusual or ambiguous queries
3. **Out of scope** — Questions outside your agent's knowledge

Iterate on your prompts based on the results.

---

## Inspiration & Examples

Check out these resources for ideas:

- [Demo Gallery](https://demo.contextual.ai) — See what's possible
- [Templates Catalog](https://docs.contextual.ai/examples/templates-catalog) — Pre-built configurations
- [Rocket Science Demo](https://docs.contextual.ai/examples/rocket_science) — Technical document analysis
- [Agent Composer Docs](https://docs.contextual.ai/quickstarts/agent-composer) — Full documentation

---

## Tips for Success

1. **Start simple** — Get a basic agent working, then add complexity
2. **Test frequently** — Don't write all your prompts before testing
3. **Be specific** — Vague prompts produce vague results
4. **Cite sources** — Grounded responses are more trustworthy
5. **Have fun** — The best projects come from genuine curiosity

---

## Resources

| Resource | Link |
|----------|------|
| Contextual AI Platform | [app.contextual.ai](https://app.contextual.ai) |
| Documentation | [docs.contextual.ai](https://docs.contextual.ai) |
| Agent Composer Guide | [YAML Guide](https://docs.contextual.ai/how-to-guides/agent-composer-yaml) |
| Demo Gallery | [demo.contextual.ai](https://demo.contextual.ai) |
| Python SDK | [SDK Reference](https://docs.contextual.ai/sdks/python) |

---

## Need Help?

Flag down a Contextual AI team member — we're here to help!

---

[Back to Event Home](./README.md)
