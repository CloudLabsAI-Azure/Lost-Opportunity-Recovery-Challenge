# Challenge 1 - Build the Intelligence Layer

## Overview

Build the intelligence backbone that powers the opportunity recovery platform. You will get historical lost opportunity data from Dynamics 365 into Azure Blob Storage, deploy AI models, build a vectorized search index from that data, and wire it all into a Foundry agent that can reason over deal records to detect loss patterns, generate revised strategies, and produce re-engagement profiles.

The challenge is complete when your Foundry agent returns grounded, cited responses about loss patterns and revised strategies in the playground.

---

## Prerequisites

Deploy all resources in the **same region** (recommended: Sweden Central or West US 2) before you start.

| Resource | Recommended SKU | Notes |
|---|---|---|
| Azure Storage Account | Standard LRS | You will create a blob container for opportunity data |
| AI Foundry Hub + Project | Standard | Create hub first, then project inside it |
| Azure OpenAI (via Foundry) | - | Deploy a chat model and an embedding model |
| Azure AI Search | Basic | Sufficient for this lab; enables semantic and vector search |

Confirm access to:
- Dynamics 365 Sales (pre-provisioned in your environment)
- Microsoft Copilot Studio
- Power Automate Premium

---

## Objectives

### 1. Opportunity Data in Blob Storage

Get your historical lost opportunity data out of Dynamics 365 and into Blob Storage so it can be indexed.

- In Dynamics 365 Sales, navigate to **Opportunities** and filter for **Closed as Lost** records. Review the pre-seeded sample data - you have at least 20 lost opportunities across multiple loss reason categories.
- Export the closed-lost opportunities to a file format suitable for downstream processing. Include the core fields (account, product line, deal value, loss reason, close date) and as much unstructured content as possible (notes, email log excerpts, call summaries).
- Create an Azure Storage Account and a blob container named `lost-opportunities`.
- Upload the exported data to the `lost-opportunities` container.

---

### 2. AI Models

Prepare the models your index and agent will rely on.

- In your AI Foundry project, deploy the following models:
  - A chat completion model - use `gpt-4.1` or `gpt-4.1-mini` (whichever is available in your region).
  - An embedding model - use `text-embedding-ada-002`.
- Note the deployment names - you will need them when configuring the index and agent.

---

### 3. Loss Intelligence Index

Build a vectorized search index from the opportunity data in Blob Storage.

- Create an Azure AI Search instance (Basic SKU).
- Use the **Import and vectorize data** option in AI Search to connect to your `lost-opportunities` blob container.
- Select your embedding model deployment as the vectorization source.
- Complete the import to create the index and run the indexer.
- Validate the index:
  - Document count is greater than 0.
  - Run test queries in Search Explorer that a sales manager would ask: "deals lost due to pricing", "competitor displacement", "approval cycle delays". Confirm relevant records come back.

---

### 4. Loss Analysis Agent

Build the AI agent in Foundry and connect it to your loss intelligence index.

- In your AI Foundry project, create a new agent using the chat model you deployed.
- Add Azure AI Search as a knowledge source, pointing to your index.
- Write a system prompt that instructs the agent to:
  - Always ground responses in indexed opportunity records - no general sales advice.
  - Cite the specific deal records used to support each finding.
  - Identify dominant loss patterns when asked about a category or account segment.
  - Generate concrete, specific revised strategies (not generic advice) grounded in what the data shows.
  - Produce a re-engagement profile for a specific lost deal, including decision-maker concerns, primary loss reason, and recommended opening for re-contact.
- Test the agent in the Foundry playground with at least five queries:
  - What are the top loss patterns in the dataset?
  - What is the revised strategy for deals lost due to pricing objections?
  - What is the revised strategy for deals lost to a named competitor?
  - Generate a re-engagement profile for [specific opportunity].
  - What is the conversion likelihood if we apply [revised strategy] to [specific deal]?

---

## Success Criteria

Before moving to Challenge 2, confirm the following:

- Closed-lost opportunity data is uploaded to the `lost-opportunities` blob container.
- Both models (chat and embedding) are deployed in your Foundry project.
- The AI Search index exists and document count is greater than 0.
- Test queries in Search Explorer return relevant opportunity records.
- The Foundry agent returns grounded, cited responses for at least 5 test queries.
- The agent produces a re-engagement profile that references the specific deal's documented concerns.

---

Click **Next** at the bottom of the page to proceed to Challenge 2.