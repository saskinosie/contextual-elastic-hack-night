# Quick Start: Create an Agent (API)

Build your first RAG agent programmatically using the Contextual AI Python SDK.

**Time to complete:** ~15 minutes

---

## Prerequisites

- Python 3.8+
- An email address to create your free Contextual AI account

---

## Step 1: Create Your Free Account & Get API Key

1. Go to [app.contextual.ai](https://app.contextual.ai)
2. Click **Sign Up** and create a free account with your email
3. Verify your email and complete the signup process
4. You'll get **$25 in free credits** ($50 if you use a work email!)

### Get Your API Key

1. Once logged in, expand the sidebar and click **API Keys**
2. Click **Create API Key**
3. Save the generated key somewhere safe — you'll need it!

---

## Step 2: Install the SDK

```bash
pip install contextual-client
```

---

## Step 3: Set Up Your Environment

Create a new Python file or notebook, then set up your client:

```python
import os
from contextual import ContextualAI

# Option 1: Set your API key as an environment variable
# export CONTEXTUAL_API_KEY="your-api-key"

# Option 2: Pass it directly (not recommended for production)
client = ContextualAI(api_key="YOUR_API_KEY")
```

> **Tip:** Never commit API keys to version control. Use environment variables or a `.env` file.

---

## Step 4: Set Up a Datastore

Your agent needs a datastore with documents to search. You have two options:

### Option A: Use a pre-processed sample datastore (fastest)

The platform includes pre-processed sample datastores ready to use. List available datastores:

```python
# List available datastores
datastores = client.datastores.list()
for ds in datastores.datastores:
    print(f"- {ds.name}: {ds.id}")
```

Pick one and save its ID for the next step.

### Option B: Upload your own documents

```python
# Create a datastore
datastore = client.datastores.create(name="My Documents")
print(f"Datastore created: {datastore.id}")

# Upload a document
with open("your-document.pdf", "rb") as f:
    result = client.datastores.documents.ingest(
        datastore_id=datastore.id,
        file=f
    )
    print(f"Document uploaded: {result.id}")
```

Wait for ingestion to complete before creating your agent. You can check status:

```python
# Check document status
metadata = client.datastores.documents.metadata(
    datastore_id=datastore.id,
    document_id=result.id
)
print(f"Status: {metadata.status}")
```

---

## Step 5: Create an Agent

Connect your agent to a datastore (use the ID from Option A or B above):

```python
# Create your agent
agent = client.agents.create(
    name="My Elastic Search Agent",
    description="A RAG agent for the Elastic Hack Night",
    datastore_ids=[datastore.id]
)

print(f"Agent created! ID: {agent.id}")
```

Save the `agent.id` — you'll need it for querying.

---

## Step 6: Query Your Agent

Now let's ask your agent a question:

```python
# Query the agent
response = client.agents.query.create(
    agent_id=agent.id,
    messages=[
        {
            "role": "user",
            "content": "What documents are available in this collection?"
        }
    ]
)

# Print the response
print("Agent Response:")
print(response.message.content)

# Print the sources (attributions)
if response.retrieval_contents:
    print("\nSources:")
    for source in response.retrieval_contents[:3]:  # Show first 3 sources
        content_preview = source.content_text[:100] if source.content_text else "(no content)"
        print(f"- {source.doc_name} (page {source.page}): {content_preview}...")
```

---

## Step 7: Customize Your Agent's Prompts

You can configure how your agent behaves by updating its settings:

```python
# Update agent with custom prompts
agent = client.agents.update(
    agent_id=agent.id,
    system_prompt="""You are a helpful search assistant powered by Contextual AI and Elasticsearch.

Your personality:
- Friendly and professional
- Concise but thorough
- Always cite your sources

When answering questions:
1. Search the datastore for relevant information
2. Provide a clear, direct answer
3. Reference the source documents
"""
)

print("Agent prompts updated!")
```

---

## Step 8: Test with Different Queries

Try a few different queries to see how your agent responds:

```python
queries = [
    "Summarize the main topics covered in these documents",
    "What are the key findings?",
    "Are there any technical specifications mentioned?"
]

for query in queries:
    print(f"\n{'='*50}")
    print(f"Query: {query}")
    print('='*50)

    response = client.agents.query.create(
        agent_id=agent.id,
        messages=[{"role": "user", "content": query}]
    )

    print(response.message.content[:500])  # Print first 500 chars
```

---

## Step 9: Submit Your Completion

You did it! Now submit your work:

1. Take a screenshot of your terminal/notebook showing a successful query
2. Fill out the [submission form](https://docs.google.com/forms/d/e/1FAIpQLSdb941rSlQ6s4EcYMu-OwiLkC_jeI-8fD181pYnsbddQnNfTA/viewform) with:
   - Your email address
   - Your screenshot
   - (Optional) Did you use GUI or API?

---

## Full Example Script

Here's everything in one script you can copy and run:

```python
import os
import time
from contextual import ContextualAI

# Initialize client
client = ContextualAI(api_key=os.environ.get("CONTEXTUAL_API_KEY"))

# Create a datastore
datastore = client.datastores.create(name="My Documents")
print(f"Datastore created: {datastore.id}")

# Upload a document
with open("your-document.pdf", "rb") as f:
    doc = client.datastores.documents.ingest(datastore_id=datastore.id, file=f)
    print(f"Document uploaded: {doc.id}")

# Wait for ingestion (simple polling)
print("Waiting for document to process...")
while True:
    metadata = client.datastores.documents.metadata(
        datastore_id=datastore.id, document_id=doc.id
    )
    if metadata.status == "completed":
        print("Document ready!")
        break
    time.sleep(5)

# Create agent
agent = client.agents.create(
    name="My Elastic Search Agent",
    description="A RAG agent for the Elastic Hack Night",
    datastore_ids=[datastore.id]
)
print(f"Agent created: {agent.id}")

# Update with custom prompt
agent = client.agents.update(
    agent_id=agent.id,
    system_prompt="""You are a helpful search assistant.
Be concise, cite your sources, and provide accurate information."""
)

# Query the agent
response = client.agents.query.create(
    agent_id=agent.id,
    messages=[{"role": "user", "content": "What is this document collection about?"}]
)

print("\nResponse:")
print(response.message.content)
```

---

## Bonus: Multi-Turn Conversations

Want to have a back-and-forth conversation with your agent? Use the `conversation_id`:

```python
# First message
response1 = client.agents.query.create(
    agent_id=agent.id,
    messages=[{"role": "user", "content": "What topics are covered?"}]
)
conversation_id = response1.conversation_id

# Follow-up message (agent remembers context)
response2 = client.agents.query.create(
    agent_id=agent.id,
    conversation_id=conversation_id,
    messages=[{"role": "user", "content": "Tell me more about the first one"}]
)

print(response2.message.content)
```

---

## Troubleshooting

### "Authentication failed"
Double-check your API key. Make sure there are no extra spaces or quotes.

### "Datastore not found"
Verify you're using the correct `datastore.id` from when you created the datastore.

### "Documents still processing"
Document ingestion can take a few minutes. Check the status before querying.

### "Rate limit exceeded"
Wait a moment and try again. If it persists, check with an organizer.

### "I ran out of credits"
The free tier includes $25-50 in credits. If you've exhausted them, let an organizer know.

### Import errors
Make sure you installed the SDK: `pip install contextual-client`

---

## Next Steps

Ready for more? Try [Build Your Own Agent](./build-your-own-agent.md) to create a custom agent with advanced features!

---

[Back to Event Home](./README.md)
