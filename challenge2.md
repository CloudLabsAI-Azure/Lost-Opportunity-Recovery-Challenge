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

<validation step="502a3add-4312-4ef6-b9a0-0bd35d454d83" />
 
> **Congratulations** on completing the Challenge! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding Challenge. If you receive a success message, you can proceed to the next Challenge. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

---

### 2. Conversation Topics

Design topics that route different types of rep queries to the right behavior.

- Create three topics covering the following scenarios:
  - **Lost Deal Analysis** - the rep provides an opportunity or account name; the agent returns the primary and contributing loss reasons detected from indexed deal content, and references comparable deals with the same loss profile.
  - **Revised Strategy** - the rep describes the loss context; the agent returns a specific, actionable revised strategy grounded in historical patterns from the index - not generic advice.
  - **Re-engagement Email** - the rep requests a draft email for a specific lost deal; the agent produces a personalized email referencing the decision-maker's documented concerns, the revised approach, and a clear call to action.
- Configure a fallback topic that surfaces a clear "not found" message rather than fabricating an answer for out-of-scope questions.
- Test each topic in the Copilot Studio canvas before publishing.

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
  - `Topic` → **Topic**
  - `Potential Customer` → **Potential Customer** (lookup resolves to **Account**)
  - `Est. Revenue` → **Est. Revenue** (select manually if shown as Not Mapped)
  - `Actual Close Date` → **Actual Close Date**
  - `Description` → **Description**
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
  - In the trigger card, locate the **Filter rows** field (it appears directly below the **Scope** field — if you don't see it, click **Show all** under **Advanced parameters** to expand it).
  - Click inside the **Filter rows** box and type the following OData expression exactly:
    ```
    statuscode eq 4
    ```
  - `statuscode eq 4` is the Dataverse code for **Status Reason = Out-Sold** on the Opportunity table. This ensures the flow only runs when a rep closes a deal as Out-Sold, and not on every other modification to the record.

- Add a **Microsoft Dataverse - Get a row by ID** action to retrieve the full opportunity record:
  - **Table name:** Opportunities
  - **Row ID:** Click inside the **Row ID** field to open the dynamic content panel. Under **When a row is added, modified or deleted**, select **Opportunity** (described as *"Unique identifier of the opportunity"*).

- Add an **AI Builder - Run a prompt** action to run AI analysis on the retrieved deal:
  - Click **+ New custom prompt** from the prompt dropdown.
  - Name the prompt `Lost Opportunity Analysis`.
  - Click inside the **Instructions** area and type the following prompt exactly as shown:

    ```
    You are a sales recovery analyst. You have been given a lost sales opportunity.

    Opportunity Topic: [Topic]
    Deal Description: [Description]

    Identify the primary loss reason from the description. Then recommend one specific, concrete re-engagement action the sales rep should take to recover this deal. Be specific to the deal — do not give generic advice.
    ```

  - In the Instructions text, select the `[Topic]` placeholder, then click **+ Add content** and add a **Text input** field named `Topic` with the following sample data:
    - **Topic sample:** `Contoso Industrial Q3 Automation Upgrade`
  - Repeat for `[Description]`: select the `[Description]` placeholder, click **+ Add content**, add a **Text input** field named `Description` with the following sample data:
    - **Description sample:** `The procurement team requested a 22% discount. The CFO placed a spending cap of $240,000 and declined to escalate for budget exception approval.`
  - Click **Test** to verify the model returns a valid response, then click **Save**.
  - Back in the flow, map the prompt inputs:
    - **Description** → select **Description** from the dynamic content panel under **Get a row by ID**
    - **Topic** → select **Topic** from the dynamic content panel under **Get a row by ID**

- Add a **Microsoft Dataverse - Add a new row** action to create a **Task** record:
  - **Table name:** Tasks
  - **Subject:** type `Lost Opportunity Analysis - ` then select **Topic** from dynamic content under **Get a row by ID**
  - **Description:** click inside the field to open the dynamic content panel, then under **Run a prompt** select **Text**.
  - **Due Date:** click the **fx** button and enter:
    ```
    addDays(utcNow(), 5)
    ```

  > **Note:** Leave the Regarding field blank.

- Save the flow and confirm there are no configuration errors, then proceed to test it using the steps below.

#### Test the Flow

- In Dynamics 365 Sales Hub, navigate to **Opportunities**.
- Open any one of the 20 imported opportunity records.
- In the command bar, select **Close as Lost**.
- In the **Close Opportunity** dialog, set **Status Reason** to **Out-Sold** and select **OK**.
- Navigate to **Power Automate**, open **Lost Opportunity Flow**, and select **Run history** to confirm the flow ran successfully with a green check.
- Return to Dynamics 365, reopen the same opportunity, and select **Activities** to confirm a new **Task** record was created with the AI-generated analysis in the Description field.

<validation step="6c0275b6-7954-4507-9d04-32e9ebd9ce83" />
 
> **Congratulations** on completing the Challenge! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding Challenge. If you receive a success message, you can proceed to the next Challenge. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

---

### 5. Publish and Validate

Publish the copilot and confirm the end-to-end system works.

- Publish the copilot to a **web channel** or **Microsoft Teams channel**.
- Open the published channel and confirm the copilot loads and responds.

<validation step="2e4b3a3b-ec54-4cce-beec-206dbeef936f" />
 
> **Congratulations** on completing the Challenge! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding Challenge. If you receive a success message, you can proceed to the next Challenge. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

---

## Success Criteria

Before submitting, confirm the following:

- The Copilot Studio agent is connected to your AI Search index and responds with grounded answers.
- The copilot is published and accessible via a live channel.
- All 20 accounts and 20 opportunities are imported into Dynamics 365 with 0 errors.
- The `Lost Opportunity Flow` runs successfully when an opportunity is closed.
- A follow-up Task is created in Dynamics 365 with AI-generated analysis in the Description.

---