# Challenge 2 - Ship the Sales Product

## Overview

Take the grounded Microsoft Foundry agent you built in Challenge 1 and give it a face that sales reps can actually use. You will create a Copilot Studio agent with conversation topics for loss analysis, strategy recommendations, and re-engagement email drafting. You will also automate the CRM so that every deal marked as lost in Dynamics 365 automatically triggers AI analysis and creates a follow-up task - no manual steps from the rep.

The challenge is complete when the copilot is live, the CRM automation flow has run successfully, and a rep can get grounded recovery guidance through a single conversation.

---

## Prerequisites

- Challenge 1 complete - your AI Search index is populated and the Microsoft Foundry agent is tested.
- Access to Microsoft Copilot Studio (included with your M365 license).
- Power Automate Premium (included in your environment).
- Dynamics 365 Sales access (pre-provisioned).

---

## Objectives

### 1. Sales Intelligence Copilot

Create the conversational interface that gives sales reps access to the Foundry agent's capabilities.

- In Microsoft Copilot Studio, verify that the **ODL environment** is selected from the environment picker before creating the agent.
- Create a new agent in Microsoft Copilot Studio with the following name:
  ```
  Sales Recovery Copilot
  ```
- Connect Azure AI Search as a knowledge source, pointing to your `lost-opportunities` index from Challenge 1.
- Configure the agent's instructions to enforce grounded, cited responses - every answer must reference specific indexed deal records, not generic sales advice.

---

### 2. Conversation Topics

Design topics that route different types of rep queries to the right behavior.

- Create three topics covering the following scenarios:
  - **Lost Deal Analysis** - the rep provides an opportunity or account name; the agent returns the primary and contributing loss reasons detected from indexed deal content, and references comparable deals with the same loss profile.
  - **Revised Strategy** - the rep describes the loss context; the agent returns a specific, actionable revised strategy grounded in historical patterns from the index - not generic advice.
  - **Re-engagement Email** - the rep requests a draft email for a specific lost deal; the agent produces a personalized email referencing the decision-maker's documented concerns, the revised approach, and a clear call to action.
- Configure a fallback topic that surfaces a clear "not found" message rather than fabricating an answer for out-of-scope questions.
- Test each topic in the Copilot Studio canvas before publishing. Every response for an in-scope query should include a citation to the source deal record.

---

### 3. Import Opportunity Data into Dynamics 365

Before building the automation, populate Dynamics 365 Sales with the 20 lost opportunity records. The Power Automate flow you build next will be tested by marking one of these records as lost.

> **Important:** You must import accounts **first**. The `Potential Customer` field on an Opportunity is a lookup to an Account record. If the Account does not exist when the opportunity row is imported, the row will fail with a lookup resolution error.

#### Step A — Import Accounts

- In Dynamics 365 Sales Hub, navigate to **Accounts**.
- Select **Import data** from the toolbar and upload `accounts.csv`.
- On the delimiter settings screen, confirm **Data Delimiter** is `Quotation mark (")` and **Field Delimiter** is `Comma (,)`, then click **Review Mapping**.
- Confirm `Account Name` maps to **Account Name** (green check on the required field), then click **Finish Import**.
- Wait for the import to complete and verify **20 Successes / 0 Errors** in **My Imports**.

#### Step B — Import Opportunities

- Navigate to **Opportunities** in Dynamics 365 Sales Hub.
- Select **Import data** from the toolbar and upload `opportunities.csv`.
- On the delimiter settings screen, confirm the same delimiter settings as above, then click **Review Mapping**.
- On the mapping screen, verify or set the following:
  - `Topic` → **Topic** ✓
  - `Potential Customer` → **Potential Customer** (lookup resolves to **Account**) ✓
  - `Est. Revenue` → **Est. Revenue** (select manually if shown as Not Mapped)
  - `Actual Close Date` → **Actual Close Date** ✓
  - `Description` → **Description** ✓
- Click **Finish Import** and verify **20 Successes / 0 Errors** in **My Imports**.
- Navigate to **My Open Opportunities** to confirm all 20 records appear with the correct Potential Customer and Est. Revenue values.

> **Note:** Fields such as Est. Close Date, Contact, Probability, and Email will be blank — this is expected. These fields are not present in the source file and are not required for the lab automation to function.

---

### 4. CRM Automation with Power Automate

Build the automation that ensures no lost deal goes unanalyzed. When a rep marks an opportunity as lost in Dynamics 365, the flow automatically runs AI analysis and creates a follow-up task.

- In Power Automate, create a new **Automated cloud flow** with the following name:
  ```
  Lost Opportunity Flow
  ```
- Set the trigger to **Microsoft Dataverse - When a row is added, modified or deleted**:
  - **Change type:** Modified
  - **Table name:** Opportunities
  - **Scope:** Organization

  > **Note:** The Opportunities table can take a few seconds to appear in the dropdown. If it does not show, wait briefly and refresh - avoid using a custom value as that may break the binding.

- Add a filter so the flow only runs when **Status Reason** equals **Lost**:
  - In the trigger's **Advanced parameters**, set **Filter rows** to:
    ```
    statuscode eq 5
    ```

- Add a **Microsoft Dataverse - Get a row by ID** action to retrieve the full opportunity record:
  - **Table name:** Opportunities
  - **Row ID:** the unique opportunity identifier from the trigger output.

- Add an action to run AI analysis on the retrieved deal content. Use either of the following, depending on what your environment supports:
  - **Option A - Foundry agent (recommended for this lab):** Use an **HTTP** action to POST to your Microsoft Foundry chat completions endpoint. Pass the opportunity **Topic** and **Description** in the request body and instruct the model to identify the primary loss reason and recommend one specific re-engagement action.
  - **Option B - AI Builder:** Use **AI Builder - Create text with GPT** with the same instructions.

  > **Note:** If AI Builder returns `EntitlementNotAvailable / NoCapacity`, your sandbox has no AI Builder credits - switch to the HTTP option (Option A) and call the Foundry endpoint directly.

- Add a **Parse JSON** action on the AI response so you can reference the generated text downstream. For a Foundry chat completion response, the analysis text is at:
  ```
  choices[0].message.content
  ```

- Add a **Microsoft Dataverse - Add a new row** action to create a **Task** record:
  - **Table name:** Tasks
  - **Subject:** `Lost Opportunity Analysis - @{Name}` (where `Name` comes from the Get a row by ID step)
  - **Description:** the AI-generated text parsed in the previous step
  - **Due Date:** `addDays(utcNow(), 5)`

  > **Note:** Do not populate any **Regarding (Opportunities)** lookup field with the raw opportunity GUID - this causes an `ODataUnrecognizedPathException` because Dataverse expects a full OData binding (`/opportunities(<id>)`), not a bare ID. Leave the Regarding field blank for this lab.

- Add a **Condition** after the AI step to check whether the generated text is empty. If empty, use **Send an email (V2)** to notify the assigned rep to run manual analysis instead of creating an empty task.

- Save and test the flow by opening one of your imported opportunities in Dynamics 365 and changing its **Status Reason** to **Lost**. Confirm a Task is created with the AI-generated analysis in the Description field.

---

### 5. Publish and Validate

Publish the copilot and confirm the full system works end-to-end.

- Publish the copilot to a **web channel** or **Microsoft Teams channel**.
- Open the published channel and confirm the copilot loads and responds.
- Run the following three validation scenarios through the live channel:
  - **Scenario A - Competitor Loss Analysis:** Provide an opportunity lost to a named competitor. Confirm the response identifies the competitor, references comparable deals, and describes the loss pattern.
  - **Scenario B - Pricing Strategy:** Select a deal lost due to pricing objection. Ask for a revised strategy. Confirm the response is specific to that deal's context - not generic cost-cutting advice.
  - **Scenario C - Re-engagement Email:** Select a high-value lost deal. Request a re-engagement email. Confirm the email addresses the decision-maker by name or role, references specific concerns from the deal record, and includes a clear call to action.
- All three responses must be grounded in indexed deal data with deal-level citations. Generic responses are not acceptable.

<validation step="6c0275b6-7954-4507-9d04-32e9ebd9ce83" />
 
> **Congratulations** on completing the Challenge! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding Challenge. If you receive a success message, you can proceed to the next Challenge. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

---

## Success Criteria

Before submitting, confirm the following:

- The Copilot Studio agent is connected to your AI Search index.
- Three conversation topics are created and tested in the canvas.
- The fallback topic returns a "not found" message for out-of-scope queries.
- The copilot is published and accessible via a live channel.
- All 20 accounts and 20 opportunities are imported into Dynamics 365 with 0 errors.
- The `Lost Opportunity Flow` runs successfully when an opportunity is closed as lost.
- A follow-up Task is created in Dynamics 365 with AI-generated analysis content in the Description.
- All three validation scenarios return grounded, cited responses through the live channel.

---

Click **Next** at the bottom of the page to proceed to the Bonus Challenge.