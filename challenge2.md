# Challenge 2 - Ship the Sales Product

## Overview

Wire the intelligence layer you built in Challenge 1 into a product sales reps can actually use. You will build a Copilot Studio agent with conversation topics for loss analysis, strategy recommendations, and re-engagement email drafting. You will also automate the CRM so that every deal marked as lost in Dynamics 365 automatically triggers AI analysis and creates a follow-up task - no manual steps from the rep.

The challenge is complete when the copilot is live, the CRM automation has run successfully, and a rep can get grounded recovery guidance through a single conversation.

---

## Prerequisites

- Challenge 1 complete - your AI Search index is populated and the Foundry agent is tested.
- Access to Microsoft Copilot Studio (included with your M365 license).
- Power Automate Premium (included in your environment).
- Dynamics 365 Sales access (pre-provisioned).

---

## Objectives

### 1. Sales Intelligence Copilot

Create the conversational interface that gives sales reps access to the Foundry agent's capabilities.

- Create a new agent in Microsoft Copilot Studio.
- Connect Azure AI Search as a knowledge source, pointing to your `lost-opportunities` index.
- Configure the agent instructions to enforce grounded, cited responses - every answer must reference specific indexed deal records, not generic sales advice.
- Create three conversation topics:
  - **Lost Deal Analysis** - the rep provides an opportunity or account name; the agent returns the primary and contributing loss reasons detected from indexed deal content, and references comparable deals with the same loss profile.
  - **Revised Strategy** - the rep describes the loss context; the agent returns a specific, actionable revised strategy grounded in historical patterns from the index - not generic advice.
  - **Re-engagement Email** - the rep requests a draft email for a specific lost deal; the agent produces a personalized email referencing the decision-maker's documented concerns, the revised approach, and a clear call to action.
- Configure a fallback topic that returns a clear "not found" message for out-of-scope queries.
- Test each topic in the Copilot Studio canvas before publishing.

---

### 2. CRM Automation with Power Automate

Build the automation that ensures no lost deal goes unanalyzed. When a rep marks an opportunity as lost in Dynamics 365, the flow automatically runs AI analysis and creates a follow-up task - no manual steps required.

- In Power Automate, create a new automated cloud flow.
- Set the trigger to **Microsoft Dataverse - When a row is added, modified or deleted**. Configure it to watch the **Opportunities** table for **Modified** rows.
  - Add a filter condition so the flow only runs when **Status Reason** equals **Lost** (value: `5`).
- Add a **Microsoft Dataverse - Get a row by ID** action to retrieve the full opportunity record using the row ID from the trigger.
- Add an **AI Builder - Create text with GPT** action. In the prompt field, compose a message that passes the opportunity **Topic** and **Description** from the previous step and instructs the model to:
  - Identify the primary loss reason based on the deal content.
  - Recommend one specific re-engagement action the rep should take first.
  - Keep the output concise - suitable for a task description field.
- Add a **Microsoft Dataverse - Add a new row** action to create a **Task** record linked to the opportunity:
  - Set **Regarding** to the opportunity record ID.
  - Set **Subject** to something like: `AI Recovery Analysis - [opportunity topic]`
  - Set **Description** to the output from the AI Builder step.
  - Set **Due Date** to 5 business days from today.
- Add a **Condition** action after the AI Builder step to check if the output is empty. If the AI call returns no content, use **Send an email (V2)** to notify the assigned rep to run manual analysis.
- Save and test the flow by opening one of your imported opportunity records in D365 and changing the **Status Reason** to **Lost**. Confirm the Task record is created and the Description contains AI-generated content.

---

### 3. Publish and Validate

Deploy the copilot and validate the complete system end-to-end.

- Publish the copilot to a web channel or Microsoft Teams channel.
- Open the channel and confirm the copilot loads and responds.
- Run the following three validation scenarios through the live channel:
  - **Scenario A - Competitor Loss Analysis:** Provide an opportunity lost to a named competitor. Confirm the response identifies the competitor, references comparable deals, and describes the loss pattern.
  - **Scenario B - Pricing Strategy:** Select a deal lost due to pricing objection. Ask for a revised strategy. Confirm the response is specific to that deal's context - not generic cost-cutting advice.
  - **Scenario C - Re-engagement Email:** Select a high-value lost deal. Request a re-engagement email. Confirm the email addresses the decision-maker by name/role, references specific concerns from the deal record, and includes a clear call to action.
- All three responses must be grounded in indexed deal data. Generic responses without deal-level citations are not acceptable.

---

## Success Criteria

Before submitting, confirm the following:

- The Copilot Studio agent is connected to your AI Search index.
- Three conversation topics are created and tested in the canvas.
- The fallback topic returns a "not found" message for out-of-scope queries.
- The copilot is published and accessible via a live channel.
- The Power Automate flow runs successfully when an opportunity is closed as lost.
- A follow-up task is created in Dynamics 365 with AI-generated analysis content in the description.
- All three validation scenarios return grounded, cited responses through the live channel.

---

Click **Next** at the bottom of the page to proceed to the Bonus Challenge.