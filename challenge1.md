# Challenge 1 - Build the Intelligence Layer

## Overview

Set up the intelligence backbone that powers the opportunity recovery platform. You will prepare opportunity data storage, deploy the AI models, build a vector search index from the deal corpus, and wire it all into a grounded Microsoft Foundry agent that can reason over lost deal records to detect loss patterns, generate revised strategies, and produce re-engagement profiles.

The challenge is complete when your Microsoft Foundry agent returns grounded, cited responses about loss patterns and revised strategies in the playground.

---

## Prerequisites

Deploy all resources in the **<inject key="Region"></inject>** region before you start.

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

<validation step="c3856564-c2db-42e0-9791-5286eeebc48d" />
 
> **Congratulations** on completing the Challenge! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding Challenge. If you receive a success message, you can proceed to the next Challenge. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

---

### 2. AI Models

Prepare the models your index and agent will rely on.

- Create a Microsoft Foundry project in the **<inject key="Region"></inject>** region.
- Once the project is deployed, select **Navigate to Foundry portal** to open it.
- In the Foundry portal, switch to the **old version** using the toggle at the top of the page.
- In your Microsoft Foundry project, deploy the following two models:
  - A chat completion model - use `gpt-4.1` or `gpt-4.1-mini`.
  - An embedding model - use `text-embedding-ada-002`.
- Note the deployment names - you will need them when configuring the index and agent.

<validation step="7291f3fd-7630-43d8-8cf0-305c73462b19" />
 
> **Congratulations** on completing the Challenge! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding Challenge. If you receive a success message, you can proceed to the next Challenge. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

---

### 3. Loss Intelligence Index

Build a vector search index from the opportunity data in Blob Storage.

- Create an Azure AI Search instance (Basic SKU).
- Use the **Import data** option in AI Search to connect to your `lost-opportunities` blob container.
- On the **Add your data** page, select your subscription, storage account, and blob container. Set **Parsing mode** to **Delimited text**, ensure **First line contains header** is selected, and keep the delimiter as `,`.
- On the **Vectorize your text** page, select the **Description** column to vectorize, set **Kind** to **Microsoft Foundry**, select your subscription and Microsoft Foundry project, and choose your **text-embedding-ada-002** model deployment.
- Set the vectorizer kind to **Microsoft Foundry**, and select your `text-embedding-ada-002` deployment. Run the indexer.

<validation step="98c45519-9031-4efe-9727-8fb4b62d50f6" />
 
> **Congratulations** on completing the Challenge! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding Challenge. If you receive a success message, you can proceed to the next Challenge. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

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