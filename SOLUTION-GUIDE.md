# SOLUTION GUIDE - Lost Opportunity Recovery Challenge

**For facilitators and proctors only. This file is not in masterdoc.json and is not visible to participants.**

---

## Architecture

```
Dynamics 365 Sales (pre-seeded data)
          |
          | export closed-lost opportunities
          v
  Azure Blob Storage
  [lost-opportunities container]
          |
          | Import and vectorize data wizard
          v
  Azure AI Search (Basic)
  [lost-opportunities-index]
          |
          | knowledge source
          v
  AI Foundry Agent
  [gpt-4.1/mini + ada-002]
          |           |
          |           | HTTP / AI Builder connector
          |           v
          |     Power Automate flow
          |     [D365 trigger: Closed as Lost]
          |     [creates CRM follow-up task]
          v
  Copilot Studio Agent
  [AI Search knowledge source]
  [3 topics: Loss Analysis, Revised Strategy, Re-engagement Email]
          |
          v
  Published Channel (Teams or Web)
```

---

## Service Roles

| Service | Role in this lab |
|---|---|
| Dynamics 365 Sales | Source of pre-seeded closed-lost opportunity records |
| Azure Blob Storage | Staging for exported opportunity data before indexing |
| Azure AI Search (Basic) | Vectorized index of opportunity data; knowledge source for agent and copilot |
| AI Foundry (Hub + Project) | Hosts gpt-4.1/mini (chat) and ada-002 (embedding); Foundry agent for reasoning |
| Copilot Studio | Conversational interface for sales reps; connects to AI Search directly |
| Power Automate Premium | Automation triggered by D365 lost opportunity event; writes tasks back to CRM |

---

## Challenge 1 - Facilitator Walkthrough

### Step 1 - Export Data from Dynamics 365

1. Sign in at `https://make.powerapps.com` with the lab credentials.
2. Select the pre-provisioned Dynamics 365 environment.
3. Navigate to **Sales > Opportunities > Closed as Lost** view.
4. Export using **Export to Excel** (static worksheet) - this gives a flat file suitable for upload.
5. For richer content: Power Automate with "Get a row" actions or the Dataverse connector can pull notes and activity history. For a quicker path, the Excel export with core fields is sufficient to test the index.

### Step 2 - Create Storage Account and Upload

1. In the Azure Portal, create a Storage Account (Standard LRS) in your resource group.
2. Create a blob container named `lost-opportunities` (private access).
3. Upload the exported file(s). Flat root is fine - no sub-folder required.

> **Proctor note:** If participants have trouble exporting from D365, a pre-prepared CSV with synthetic opportunity data (20 records, 4 loss categories) is acceptable as a substitute. The AI Search index and agent behaviour will be the same.

### Step 3 - Deploy AI Models in Foundry

1. Navigate to `https://ai.azure.com`.
2. Create a Hub and a Project if not pre-provisioned.
3. In the project, go to **Models + Endpoints > Deploy model**.
4. Deploy `gpt-4.1` or `gpt-4.1-mini` - name the deployment `gpt-4-chat`.
5. Deploy `text-embedding-ada-002` - name the deployment `ada-embedding`.

> **Proctor note:** Model availability varies by region. If gpt-4.1 is not available, gpt-4o is an acceptable substitute. ada-002 is universally available.

### Step 4 - Build the Index with Import and Vectorize

1. In the Azure AI Search resource, select **Import and vectorize data**.
2. Source: Azure Blob Storage. Select the `lost-opportunities` container.
3. Vectorize text: select your Foundry embedding model deployment (`ada-embedding`).
4. Index name: `lost-opportunities-index`.
5. Complete the wizard and wait for the indexer run to finish.
6. In **Search Explorer**, run a query: `lost due to pricing`. Confirm records return.

### Step 5 - Create the Foundry Agent

1. In the AI Foundry project, go to **Agents** and create a new agent.
2. Select the chat model deployment (`gpt-4-chat`).
3. Under **Knowledge**, add Azure AI Search and select `lost-opportunities-index`.
4. Paste this system prompt (adjust as needed):

```
You are a sales intelligence assistant for a manufacturing sales team.
Your job is to analyze historical lost opportunities from the sales CRM and help sales managers understand why deals were lost and what they should do differently.

Rules:
- Always ground your answers in the indexed opportunity records. Do not give generic sales advice.
- When identifying loss patterns, cite the specific deals that support each pattern.
- When generating revised strategies, make them specific to the loss category, product line, or account context - not generic.
- When producing a re-engagement profile, include: the decision-maker's documented concerns, the primary loss reason, the proposed revised approach, and the recommended opening for re-engagement contact.
- Format all responses clearly with headers. Keep language readable for a non-technical sales rep.
```

5. Test with these queries in the playground:
   - "What are the top loss patterns in the dataset?"
   - "What is the revised strategy for deals lost due to pricing?"
   - "Generate a re-engagement profile for [opportunity name]."
   - "What is the conversion likelihood if we lead with flexible payment terms for [account]?"

---

## Challenge 2 - Facilitator Walkthrough

### Step 5 - Create the Copilot Studio Agent

1. Navigate to `https://copilotstudio.microsoft.com`.
2. Create a new agent. Name it "Sales Recovery Assistant".
3. Under **Knowledge**, add Azure AI Search.
4. Use the same `lost-opportunities-index` from Challenge 1.
5. In agent instructions, paste (or adapt):

```
You are a sales recovery assistant for a manufacturing sales team.
You help sales reps understand why they lost deals and what to do to recover them.
Always cite specific deals from the knowledge base. Do not give generic advice.
If you cannot find relevant deal data, say so clearly.
```

6. Create three topics manually:
   - **Lost Deal Analysis** - trigger phrases: "analyze deal", "why did we lose", "loss analysis for [deal]"
   - **Revised Strategy** - trigger phrases: "revised strategy", "what should we do differently", "strategy for"
   - **Re-engagement Email** - trigger phrases: "draft email", "re-engagement email", "write email to"

> **Proctor note:** Each topic should use a Send Message node with a generative answer action pointing to the AI Search knowledge source. Copilot Studio's generative answers feature handles grounding automatically when a knowledge source is connected.

### Step 6 - Power Automate Flow

1. Navigate to `https://make.powerautomate.com`.
2. Create a new **Automated cloud flow**.
3. Trigger: **Microsoft Dataverse - When a row is added, modified, or deleted**
   - Table: Opportunities
   - Change type: Modified
   - Filter: Status Reason = Closed as Lost (value 5 in standard D365)
4. Action 1: **Dataverse - Get a row** - retrieve the full opportunity record.
5. Action 2: **HTTP** - POST to your Foundry agent endpoint (or use **AI Builder - Generate text with a GPT model** if available).
   - Body: pass opportunity name, account, notes, and loss reason from the trigger.
6. Action 3: **Parse JSON** - extract primary loss reason and strategy from the AI response.
7. Action 4: **Dataverse - Add a new row** (Task table)
   - Regarding: opportunity record from step 4
   - Subject: "AI Recovery Analysis - [Opportunity Name]"
   - Description: AI analysis output
   - Due Date: 5 business days from close date (use `addDays(triggerOutputs()?['body/actualclosedate'], 7)` as an approximation)
8. Add a **Scope + Configure run after** block for error handling - on failure, send a Teams or email notification to the assigned rep.
9. Test: change a test opportunity to Closed as Lost in D365 and confirm the task appears.

> **Proctor note:** The Foundry agent HTTP endpoint is found in AI Foundry under **Agents > [agent name] > Endpoint**. The API key is in the same panel. For a simpler path without Foundry HTTP, use Power Automate's built-in **AI Builder - Create text with GPT** action and pass the opportunity context as the prompt.

### Step 7 - Publish and Validate

1. In Copilot Studio, go to **Publish** and publish the agent.
2. Under **Channels**, select **Web** or **Microsoft Teams**.
   - For Teams: follow the in-product steps to create a Teams app package and install to a team.
   - For Web: copy the iframe embed code - open in a browser to confirm it loads.
3. Run the three validation scenarios through the live channel:
   - Scenario A: "Analyze the deal with [competitor name] - why did we lose?"
   - Scenario B: "What is the revised strategy for deals lost on pricing in the industrial equipment line?"
   - Scenario C: "Draft a re-engagement email for [high-value opportunity name]."
4. Confirm all three return grounded responses with deal citations.
5. Trigger the Power Automate flow by updating a D365 opportunity to Closed as Lost and confirm task creation.

---

## Common Issues and Resolutions

| Issue | Resolution |
|---|---|
| AI Search indexer returns 0 documents | Check blob container access - use "Storage Blob Data Reader" role or allow shared key access. Re-run the indexer manually from the Indexers panel. |
| Foundry agent returns generic responses | Check that AI Search is listed under Knowledge in the agent configuration. If the index is empty the agent falls back to model knowledge - re-run the indexer. |
| Copilot Studio generative answers not grounded | Confirm the AI Search knowledge source is added under the agent's Knowledge panel, not just in a topic. The knowledge source must be at agent level for generative answers to use it. |
| Power Automate D365 trigger not firing | Confirm the filter row condition is correct. In standard D365, Closed as Lost = Status Reason value 5. Check the flow's trigger condition expression. |
| HTTP call to Foundry agent returns 401 | The Foundry agent endpoint requires an Authorization header. Use the API key from AI Foundry Agents panel as a Bearer token. Store it in a Power Automate environment variable, not hardcoded. |
| D365 task not created | Check the Task table name in Dataverse. Use "Tasks" not "Task". Confirm the Regarding field is set using the opportunity ID from the trigger. |
| Re-engagement email is generic | The AI Search index needs rich unstructured content from the opportunity notes and emails. If the source export only had core fields, re-export with notes included and re-run the indexer. |
