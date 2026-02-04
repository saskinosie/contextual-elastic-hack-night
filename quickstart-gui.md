# Quick Start: Create an Agent (GUI)

Build your first RAG agent using the Contextual AI Platform UI — no code required.

**Time to complete:** ~10 minutes

---

## Prerequisites

- An email address to create your free account

---

## Step 1: Create Your Free Account

1. Go to [app.contextual.ai](https://app.contextual.ai)
2. Click **Sign Up** and create a free account with your email
3. Verify your email and complete the signup process
4. You'll get **$25 in free credits** ($50 if you use a work email!)

<!-- Screenshot: Signup page -->

---

## Step 2: Create a Datastore (Optional)

If you want to use your own documents, create a datastore first. Otherwise, skip to Step 3 and use the **Quick Start with Sample Data** option.

1. From the sidebar, click **Datastores**
2. Click **Create Datastore**
3. Give it a name (e.g., "My Documents")
4. Upload your documents (PDFs, TXT, DOCX)
5. Wait for ingestion to complete (check the status indicator)

<!-- Screenshot: Create datastore -->

---

## Step 3: Create Your Agent

1. From the sidebar, click **Agents**
2. Click **Create Agent** in the upper right
3. Fill in the **General** information:
   - **Agent Name:** Give your agent a name (e.g., "My Search Agent")
   - **Description:** Brief description of what your agent does

4. Under **Configuration**, choose how you want to set up your agent:

   - **Template** (recommended) — Get started quickly with pre-built agents
     - Select a **Pre-Built** template: Agentic Search, Simple Search, Deep Research, etc.

   - **Prompt** (Enterprise) — Describe what you want and we'll generate a custom configuration

   - **Blank Canvas** (Enterprise) — Build from scratch with full control using Agent Composer

   For this quickstart, select **Template** → **Agentic Search**

5. Under **Datastores**, you have three options:
   - **Quick Start with Sample Data** (fastest) — Explore immediately with no upload or wait
   - **Create new datastore** — Upload your own documents
   - **Link with existing datastore** — Use a datastore you've already created

6. Choose how to proceed:
   - **Start Chatting** — Create your agent and jump straight into querying
   - **Create and Customize** — Create your agent and configure prompts before chatting

<!-- Screenshot: Create agent form -->

---

## Step 4: Chat with Your Agent

Your agent is ready! Start asking questions and see how it responds.

**Want to customize your agent later?** You can edit it anytime:
1. From the sidebar, click **Agents**
2. Find your agent and click the **⋮** (hamburger menu) next to it
3. Select **Edit**

---

## Step 5: Configure Your Agent Prompts (Optional)

If you clicked **Create and Customize**, you'll see the **Agent Composer** — a visual workflow editor for your agent.

### Using Agent Composer

The workflow shows how your agent processes queries:

**Input Node** → **Create Message History** → **Agentic Research Step** → **Generate From Research Step** → **Output Node**

To configure any node, click on it and select the **Configure** button that appears above the node.

### Key Settings to Customize

In the **Agentic Research Step** node, you can edit:

| Setting | What It Does |
|---------|--------------|
| **Model** | Choose the LLM (e.g., Claude Sonnet 4.5) |
| **Identity Guidelines Prompt** | Define who the agent is and how it behaves |
| **Research Guidelines Prompt** | Control when and how the agent searches your datastore |

**Example Identity Guidelines:**
```
You are a helpful search assistant powered by Contextual AI and Elasticsearch.
You provide factual, grounded answers by retrieving information from documents.

Your personality:
- Friendly and professional
- Concise but thorough
- Always cite your sources
```

**Example Research Guidelines:**
```
You have access to the following tool:
- `search_docs` — Search the document datastore

Search the datastore when:
- The user asks a factual question
- You need to verify information
- The user requests specific details from documents

Always retrieve relevant context before answering.
```

### Model Armor (Safety Filters)

You can also configure safety filters like:
- CSAM Filter
- Malicious URIs Filter
- PI and Jailbreak Filter
- Hate Speech Filter

<!-- Screenshot: Prompt editing interface -->

---

## Step 6: Test Your Agent

1. From the sidebar, click **Agents**
2. Select your agent to open the chat interface
3. Enter a test query related to the datastore content
4. Review the response and the retrieved sources

**Example queries to try:**
- "What documents are in this collection?"
- "Summarize the main topics covered"
- [Add domain-specific queries based on the datastore content]

<!-- Screenshot: Query interface with response -->

---

## Step 7: Submit Your Completion

You did it! Now submit your work:

1. Take a screenshot of your agent's query response
2. Fill out the [submission form](https://docs.google.com/forms/d/e/1FAIpQLSdb941rSlQ6s4EcYMu-OwiLkC_jeI-8fD181pYnsbddQnNfTA/viewform) with:
   - Your email address
   - Your screenshot
   - (Optional) Did you use GUI or API?

---

## Bonus: Experiment!

Try these tweaks to see how they affect your agent:

| Experiment | What to Try |
|------------|-------------|
| **Tone** | Change the identity prompt to be more formal or casual |
| **Format** | Ask for bullet points, tables, or numbered lists in response guidelines |
| **Focus** | Add constraints like "focus on technical details" or "explain like I'm new to this" |

---

## Troubleshooting

### "My documents are still processing"
Document ingestion can take a few minutes depending on file size. Check the status indicator in your datastore.

### "My agent isn't returning good results"
- Try rephrasing your query
- Check that your prompts don't conflict with each other
- Make sure the datastore contains relevant content for your question
- Ensure your documents finished ingesting

### "I got an error"
Take a screenshot of the error and flag down a Contextual AI team member.

### "I ran out of credits"
The free tier includes $25-50 in credits. If you've exhausted them, let an organizer know.

---

## Next Steps

Ready for more? Try [Build Your Own Agent](./build-your-own-agent.md) to create a custom agent from scratch!

---

[Back to Event Home](./README.md)
