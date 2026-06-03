# One Hackathon - Recommendations on Lost/Dropped Opportunities with Revised Strategy Suggestions

Welcome to the Hack to Skill: Lost Opportunity Recovery challenge.

In this hackathon, you will design and build an AI-powered sales intelligence platform that analyzes historical lost and dropped sales opportunities, detects patterns and root causes, generates revised strategies, drafts personalized re-engagement messages, and automates follow-up workflows directly inside the CRM - helping manufacturing sales teams recover lost deals through data-driven intelligence.

---

## Problem Statement

Manufacturing sales teams lose significant revenue to preventable deal failures. Pricing objections, delayed follow-ups, competitor displacement, incomplete proposals, and long approval cycles all contribute - but the data that would explain why each deal was lost is scattered across Dynamics 365 CRM activity logs, email threads, call notes, and opportunity history.

Without a way to aggregate and analyze this information at scale, sales managers rely on anecdotal knowledge and gut instinct to course-correct. The patterns exist in the data. The problem is that no one has the time or tools to find them.

This challenge builds the AI layer that makes those patterns visible. Your system must ingest historical opportunity data and communication records, extract structured intelligence from unstructured content, identify recurring loss reasons, generate actionable revised strategies for each loss category, draft personalized re-engagement emails for decision-makers, predict conversion likelihood when a revised strategy is applied, and automate the creation of follow-up tasks and meeting requests directly in the CRM.

---

## About the Sandbox Environment

   | Resources | Value | Remarks |
   | --- | --- | --- |
   | Enabled Services | `Azure AI Search` <br> `Microsoft AI Foundry` <br> `Azure Document Intelligence` <br> `Azure Blob Storage` <br> `Dynamics 365 Sales Enterprise` <br> `Microsoft 365 Business Basic` <br> `Power Automate Premium` <br> `Microsoft Copilot Studio` | You have full access over the Azure subscription and M365 tenant. Use these services to build the end-to-end opportunity recovery platform. |
   | Azure Entra ID User | Pre-created Entra ID user account | You will get one Entra ID User Account with access to all required services. |
   | Azure Subscription Permissions | **Owner** privilege over Azure Subscription | You will get Owner access to the Azure subscription. |
   | Azure Credit | **$50 USD** | Consumption limit is set on Azure spend to 50 USD. |
   | Credit Alerts | Credit Alerts are set on consumption of 25%, 50%, 75%, 85%, 90%, 95% and 100% of total Azure credits. | Make sure to check your registered email's inbox for any alert-related mails. Alerts give you a head start to keep your Azure spending in control and to plan out the remaining credits in the best way possible. |
   | Sandbox Duration | 8 Hours or until Azure Consumption Credits are exhausted. | The sandbox environment will be deleted automatically after 8 Hours or once the Azure credits are exhausted, whichever comes first. |

---

## Why This is Enterprise-Ready - Not Just a CRM Report

Sales leaders already have CRM dashboards that show win/loss ratios. What you are building here is categorically different - it reasons over the content of deals, not just the counts.

The table below shows what separates an AI-powered opportunity recovery platform from a standard CRM analytics report:

| Capability | Standard CRM Reporting | What You Build Here |
| --- | --- | --- |
| Data source | Structured CRM fields (stage, amount, close date) | CRM fields plus unstructured notes, emails, call logs, and proposal documents extracted and indexed |
| Loss reason detection | Manually tagged by rep (often incomplete or absent) | AI-detected from free-text notes, email threads, and activity history using Document Intelligence and AI Search |
| Pattern analysis | Count-based reports (top loss reasons by volume) | Semantic clustering of loss themes across deals, surfacing combinations of factors that rarely appear in structured fields |
| Strategy generation | None - rep experience only | Foundry agent generates revised strategies per loss category based on historical win patterns in similar contexts |
| Re-engagement | Manual email drafting by rep | Personalized re-engagement email drafted by the agent, grounded in the specific loss reasons and decision-maker context for each deal |
| Conversion prediction | Not available | Likelihood score when a revised strategy is applied, based on historical win/loss patterns in comparable scenarios |
| CRM automation | Rep creates tasks manually after analysis | Power Automate triggers on a lost opportunity event in Dynamics 365, runs analysis, and writes follow-up tasks and meeting requests back into the CRM automatically |
| Interface | CRM dashboard only | Copilot Studio copilot accessible to sales reps for on-demand analysis, strategy queries, and email drafting |

This platform is designed to be connected to a live Dynamics 365 tenant and used by sales managers and reps as part of their daily workflow - not as a one-time report.

---

## Best Practices:

+ **Resources usage:** Please stop or scale down Azure AI Search, Document Intelligence, and AI Foundry resources when not in use to minimize Azure spend.
+ **Azure Cost Analysis:** Maintain a practice of regularly checking the Cost Analysis report for the assigned Azure subscription to ensure sustainability of the environment over the challenge duration.
+ **Alert notifications:** Make sure to check your registered email's inbox for any alert-related emails. Alerts give you a head start to keep your Azure spending in control.
+ **Dataset scope:** Keep your opportunity dataset focused and representative. A well-chosen set of 20-30 historical opportunities with varied loss reasons is more useful than a large dataset with uniform patterns.
+ **Dynamics 365 data:** Use the pre-seeded sample opportunity data provided in your environment. You do not need to manually create all records from scratch.

---

## Hack to Skill Format: Challenge-Based

This hackathon is structured into two progressive challenges that build the opportunity recovery platform in two distinct phases. The first challenge focuses on the intelligence backend. The second challenge focuses on the sales-facing product and CRM automation layer built on top of it.

- **Challenge 1 - Intelligence Pipeline: Extraction, Indexing & Loss Pattern Analysis**  
  Export and ingest historical opportunity data and communication records from Dynamics 365 into Blob Storage, extract structured intelligence from unstructured content using Document Intelligence, build a semantic AI Search index that captures deal attributes and loss signals, and develop a Microsoft AI Foundry agent capable of detecting loss patterns, generating revised strategies, and predicting conversion likelihood. This challenge delivers the intelligence layer.

- **Challenge 2 - Sales Copilot Experience: Interface, Re-engagement & CRM Automation**  
  Wire the Foundry agent to a Copilot Studio copilot with conversation flows for loss analysis, strategy recommendations, and re-engagement email drafting. Build a Power Automate flow that triggers automatically on a lost opportunity event in Dynamics 365 and writes analysis results, follow-up tasks, and meeting requests back into the CRM. Validate the complete system end-to-end with real scenarios. This challenge delivers the finished product sales teams can use.

Challenge 1 must be completed before starting Challenge 2. The platform is not complete until the Copilot Studio copilot is operational, the CRM automation flow has run successfully, and a sales rep can receive actionable recovery guidance with a single conversation.

---

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year via email and live chat to ensure seamless assistance throughout the hackathon.

**Learner Support Contacts**  
- Email: cloudlabs-support@spektrasystems.com  
- Live Chat: https://cloudlabs.ai/labs-support

Click **Next** to proceed to set up the prerequisites for this hackathon.
