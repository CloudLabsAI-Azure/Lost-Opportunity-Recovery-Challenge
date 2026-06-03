## Day 01 - Morning

# Challenge 1: Intelligence Pipeline - Extraction, Indexing & Loss Pattern Analysis

## Goal

Build the intelligence backend of the opportunity recovery platform - the layer that turns scattered CRM data into actionable sales intelligence. You will ingest historical opportunity records and communication content from Blob Storage, extract structured deal intelligence from unstructured notes and emails, design a semantic knowledge index optimized for loss pattern retrieval, and develop a Microsoft AI Foundry agent that identifies root causes of deal failures, generates revised strategies, and predicts the likelihood of conversion when a new approach is applied.

This challenge delivers the AI reasoning layer. No sales-facing interface yet - just clean data, precise retrieval, and an agent you can test directly in the Foundry playground.

---

## Challenge Overview

Every lost deal contains a story. The loss reason field in the CRM tells you one word. The notes, emails, and activity log tell you the full picture. The challenge here is to read all those stories at scale and find what they have in common.

In this challenge, you will build the intelligence foundation. You will extract structure from messy, freeform sales content, design an index that captures the signals that matter - pricing context, competitor mentions, timing patterns, approval chain complexity - and build an agent that can reason across that indexed knowledge to tell a sales manager not just what went wrong, but what to do differently.

In this challenge, you will:

- Ingest exported opportunity records and communication content into a structured extraction pipeline using Azure Document Intelligence.
- Design and populate an Azure AI Search index that captures deal attributes, loss signals, and contextual metadata in a format suitable for semantic and vector retrieval.
- Build a Microsoft AI Foundry agent grounded in the index, capable of identifying loss patterns, generating revised strategies per loss category, and predicting conversion likelihood.

The challenge is complete when your Foundry agent can accurately identify the dominant loss patterns in the dataset, produce a concrete revised strategy for at least two distinct loss categories, and generate a conversion likelihood assessment for a specified re-engagement scenario - all tested directly in the Foundry playground.

---

## Objectives

### 1. Opportunity Data Ingestion & Structured Extraction

Build the data foundation by ingesting exported opportunity content into Azure Blob Storage and extracting structured, machine-readable intelligence from unstructured CRM records.

- Confirm your exported Dynamics 365 opportunity data is organized in Blob Storage. Verify that at minimum the following content types are present for each deal: core opportunity fields, associated notes and description text, and any exported email or call log content.
- Use Azure Document Intelligence to process the opportunity records and communication content. Your extraction must produce a consistent schema per opportunity that captures:
  - Deal identifiers: opportunity name, account, product line, deal value, owner
  - Outcome metadata: close date, loss reason (field), deal stage at loss
  - Unstructured content: combined notes text, email thread excerpts, call log summaries
  - Extracted signals: competitor names mentioned, pricing objections raised, timeline or urgency language, decision-maker references, product fit concerns
- Define signal extraction logic that identifies and tags the presence of key loss indicators from free text. Consider how you will distinguish between explicit statements ("competitor offered lower price") and implicit signals ("evaluation extended three times").
- Validate extraction quality across at minimum five opportunity records spanning at least two different loss reason categories. Assess whether the extracted schema faithfully captures the loss story from each record.

Your extraction must be schema-consistent. All records fed into the downstream index must conform to the same structure, regardless of how sparse or detailed the source content is.

Outcome:
A structured, validated dataset of extracted opportunity intelligence is ready for indexing, covering deal attributes, loss signals, and unstructured communication content in a consistent schema across all records.

---

### 2. Loss Intelligence Index Construction

Design and build an Azure AI Search index optimized for semantic retrieval of sales loss patterns, deal contexts, and outcome signals.

- Define an index schema that captures the extracted fields from Objective 1 alongside the following metadata needed for downstream agent reasoning:
  - Loss reason category (both the CRM field value and AI-detected categories from text)
  - Detected competitor presence and named competitors where extractable
  - Price sensitivity signal (boolean or score)
  - Decision-maker engagement level (based on email thread depth, meeting count, or similar)
  - Chunk-level references back to the source opportunity record for citation
- Configure semantic search on the index to support natural language queries about loss patterns. A query like "deals lost due to pricing with long approval cycles" must surface relevant records without requiring exact field matches.
- Integrate vector embeddings on the notes and combined text fields to enable similarity search across deals. This allows the agent to find deals that are contextually similar to a new at-risk opportunity even when the exact same loss keywords are not present.
- Populate the index with all extracted opportunity records.
- Evaluate retrieval quality by running at least five representative queries that reflect real questions a sales manager would ask: "Which accounts were lost to competitor X?", "What patterns appear in deals that stalled at the proposal stage?", "Which product lines have the highest pricing objection rate?". Assess and tune retrieval precision.

Outcome:
A production-quality Azure AI Search index is operational, supports semantic and vector retrieval across opportunity data, and returns relevant results for representative sales intelligence queries with source traceability back to individual deals.

---

### 3. Foundry AI Agent for Loss Analysis, Strategy Generation & Prediction

Build the reasoning layer in Microsoft AI Foundry - an agent grounded in the loss intelligence index that can detect patterns, generate strategy recommendations, and predict conversion likelihood.

- Create or configure a project in Microsoft AI Foundry connected to your Azure AI Search index.
- Design and implement an agent that retrieves relevant opportunity data from the index before reasoning over it. The agent must not rely on parametric knowledge for domain-specific sales pattern analysis - all conclusions must be grounded in indexed deal data.
- Engineer prompts and reasoning flows that deliver the following capabilities:

  **Loss Pattern Detection:** Given a query about a loss category, account segment, or product line, the agent must identify the dominant patterns across relevant deals, rank them by frequency and impact, and reference the specific deals that support each pattern.

  **Revised Strategy Generation:** For each identified loss pattern, the agent must generate at least one concrete revised strategy. Strategies must be specific and actionable - not generic advice. For example: "The three deals lost to Competitor X in the mid-market segment were all competitive on feature set but lost on payment terms. A revised strategy for similar deals is to proactively introduce flexible payment scheduling in the proposal stage, referencing the account's capital expenditure cycle." Strategies should be grounded in what historically worked in similar won deals if that data is available in the index.

  **Conversion Likelihood Prediction:** Given a specific lost opportunity and a proposed revised strategy, the agent must produce a likelihood assessment (expressed as High/Medium/Low with a confidence rationale) based on how similar scenarios resolved historically. The rationale must cite the comparable deals used to generate the assessment.

  **Re-engagement Profile:** For a specified lost opportunity, the agent must produce a structured profile that captures the decision-maker's stated concerns, the primary loss reason, the proposed revised approach, and the recommended opening framing for re-engagement contact - suitable for use as input to email drafting in Challenge 2.

- Test the agent against at least six queries covering loss pattern analysis, strategy generation for two different loss categories, conversion likelihood for two specific deals, and a re-engagement profile for one deal. Document which responses are accurate, grounded, and actionable, and which reveal gaps in the index or prompting.

Outcome:
A Microsoft AI Foundry agent delivers grounded, cited sales intelligence across loss pattern analysis, revised strategy generation, conversion likelihood prediction, and re-engagement profiling - verified through direct testing in the Foundry playground.

---

## Expected Outcomes

By the end of Challenge 1:

- An Azure Blob Storage structure holds exported Dynamics 365 opportunity data and communication content organized for extraction.
- Azure Document Intelligence extracts structured deal intelligence from unstructured records using a repeatable, schema-consistent pipeline.
- An Azure AI Search index with semantic and vector search is operational, captures loss signals and deal attributes, and returns relevant results for representative sales intelligence queries.
- A Microsoft AI Foundry agent is grounded in the index and delivers accurate, cited responses for loss pattern detection, revised strategy generation, conversion likelihood prediction, and re-engagement profiling.
- Direct agent testing in the Foundry playground confirms grounding quality, actionability of strategy outputs, and correct handling of queries about deals not in the indexed dataset.

---

## Completion Criteria (Mandatory Submission)

Please use the instructions provided below and follow the submission steps carefully:

Once you complete this challenge, you must:

1. Keep the below artifacts ready to be uploaded:

   - A screenshot of the Azure AI Search index showing the populated index with document count and field schema visible - confirming that loss signal fields and vector-enabled text fields are present.
   - A screenshot of the Microsoft AI Foundry agent playground showing a loss pattern analysis response that identifies at least two distinct loss patterns and references specific opportunity records as evidence.

1. Name the screenshots using the below naming convention:

   - `<Your_Name>_<Challenge01>_<file01>_<Time_Stamp(HH:MM)>`
   - `<Your_Name>_<Challenge01>_<file02>_<Time_Stamp(HH:MM)>`

1. Navigate back to **Hackathon Portal** where you registered for the hackathon.

1. In the hackathon portal, select **Learning Resources** page.

   ![](./media/hackportalv2.png)

1. Scroll down to bottom, under **Upload Your Certificate**, click **upload Certificate** and upload the artifacts that you have prepared earlier.

   ![](./media/hack2.png)

This submission is mandatory.

Failure to submit these artifacts will result in the challenge being marked as incomplete.

## Congratulations! You have successfully completed Challenge 1
