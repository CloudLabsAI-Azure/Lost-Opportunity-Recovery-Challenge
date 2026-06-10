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

Build the automation that ensures no lost deal goes unanalyzed.

- Create a Power Automate Premium flow with a Dynamics 365 trigger that fires when an opportunity is updated to **Closed as Lost**.
- Configure the flow to:
  - Retrieve the full opportunity record including notes and activity history.
  - Call the AI Search index (or the Foundry agent via HTTP/AI Builder) to run loss analysis on the retrieved deal data.
  - Create a follow-up task in Dynamics 365 linked to the opportunity, with the AI-generated analysis summary and recommended first re-engagement action in the task description. Set a due date of 5 business days from close date.
- Add error handling: if the AI call fails, do not create a blank task - log the failure and notify the rep to trigger analysis manually.
- Test the flow by changing a test opportunity status to Closed as Lost. Confirm the follow-up task is created in Dynamics 365 with the AI content populated.

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

## Submission

Take three screenshots and keep them ready for upload:

1. The copilot in the live channel (URL bar or Teams context visible) showing a grounded response to one of the three validation scenarios.
2. The Power Automate flow run history showing a successful run triggered by a Dynamics 365 status change.
3. A Dynamics 365 opportunity record showing the automatically created follow-up task with AI analysis content in the task description.

Name your files: `<Your_Name>_Challenge02_<file01>_<HH:MM>`, `<file02>`, `<file03>` (same pattern).

Navigate to the **Hackathon Portal > Learning Resources**, scroll to **Upload Your Certificate**, and submit all three files.

---

Click **Next** at the bottom of the page to proceed to the Bonus Challenge.