# Challenge 1 - Build the Intelligence Layer

## Overview

Set up the intelligence backbone that powers the opportunity recovery platform. You will prepare opportunity data storage, deploy the AI models, build a vector search index from the deal corpus, and wire it all into a grounded Microsoft Foundry agent that can reason over lost deal records to detect loss patterns, generate revised strategies, and produce re-engagement profiles.

The challenge is complete when your Microsoft Foundry agent returns grounded, cited responses about loss patterns and revised strategies in the playground.

---

## Prerequisites

Deploy all resources in the **same region** (recommended: sweden central, west us 2, ) before you start.

| Resource | Recommended SKU | Notes |
|---|---|---|
| Azure Storage Account | Standard LRS | You will create a blob container for the opportunity dataset |
| Foundry Project | Standard | Create a Microsoft Foundry Project |
| Azure AI Search | Basic | Sufficient for this lab; enables semantic and vector search |

Confirm access to:

- Dynamics 365 Sales (pre-provisioned in your environment)
- Microsoft Copilot Studio
- Power Automate Premium

---

## Objectives

### 1. Opportunity Data Storage

Set up the storage layer that holds the lost opportunity corpus.

- Create an Azure Storage Account and a blob container named `lost-opportunities`.
- Locate the `opportunities.csv` file you downloaded in Getting Started. This file contains 20 closed-lost opportunity records across five loss reason categories, with rich deal narratives in the Description field.
- Upload `opportunities.csv` to the `lost-opportunities` container.

> **Note:** In a production scenario this data would be exported directly from Dynamics 365. For this lab the dataset is pre-prepared so you can focus on building the intelligence layer.

---

### 2. AI Models

Prepare the models your index and agent will rely on.

- In your Microsoft Foundry project, deploy the following two models:
  - A chat completion model - use `gpt-4.1` or `gpt-4.1-mini` (whichever is available in your region).
  - An embedding model - use `text-embedding-ada-002`.
- Note the deployment names - you will need them when configuring the index and agent.

---

### 3. Loss Intelligence Index

Build a vector search index from the opportunity data in Blob Storage.

- Create an Azure AI Search instance (Basic SKU).
- Use the **Import data** option in AI Search to connect to your `lost-opportunities` blob container.
- Select your embedding model deployment as the vectorization source.
- Complete the wizard to create the index and run the indexer.
- Validate the index:
  - Document count is greater than 0.
  - Run a few test queries a sales manager would ask in Search Explorer - for example, "deals lost due to pricing", "competitor displacement", "approval cycle delays". Confirm relevant records come back.

---

### 4. Loss Analysis Agent

Build the AI agent in Microsoft Foundry and connect it to your loss intelligence index.

- In your Microsoft Foundry project, create a new agent using the chat model you deployed.
- Add Azure AI Search as a knowledge source on the agent, pointing to your index.
- Write a system prompt that instructs the agent to:
  - Always ground responses in indexed opportunity records - no general sales advice.
  - Cite the specific deal records used to support each finding.
  - Identify dominant loss patterns when asked about a category or account segment.
  - Generate concrete, specific revised strategies (not generic advice) grounded in what the data shows.
  - Produce a re-engagement profile for a specific lost deal, including decision-maker concerns, primary loss reason, and recommended opening for re-contact.
- Test the agent in the Microsoft Foundry playground with at least five queries covering different scenarios:
  - What are the top loss patterns in the dataset?
  - What is the revised strategy for deals lost due to pricing objections?
  - What is the revised strategy for deals lost to a named competitor?
  - Generate a re-engagement profile for [specific opportunity].
  - What is the conversion likelihood if we apply [revised strategy] to [specific deal]?

---

## Success Criteria

Before moving to Challenge 2, confirm the following:

- The `opportunities.csv` dataset is uploaded to the `lost-opportunities` blob container.
- Both models (chat and embedding) are deployed in your Microsoft Foundry project.
- The AI Search index exists and document count is greater than 0.
- Test queries in Search Explorer return relevant opportunity records.
- The Microsoft Foundry agent returns grounded, cited responses for at least 5 test queries.
- The agent produces a re-engagement profile that references the specific deal's documented concerns.

---

Click **Next** at the bottom of the page to proceed to Challenge 2.