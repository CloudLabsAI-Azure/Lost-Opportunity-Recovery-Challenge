## Getting Started with Challenge

We've prepared a seamless environment for you to explore and learn. Let's begin by making the most of this experience.

### Accessing Your Challenge Environment

Once you're ready to dive in, your virtual machine and challenge guide will be right at your fingertips within your web browser.

![](./media/gs1.png)

### Exploring Your Challenge Resources

To get a better understanding of your challenge resources and credentials, navigate to the Environment tab.

![](./media/gs-leave-2.png)

### Utilizing the Split Window Feature

For convenience, you can open the challenge guide in a separate window by selecting the Split Window button from the Top right corner.

![](./media/gs-leave-3.png)

### Managing Your Virtual Machine

Feel free to start, stop, or restart your virtual machine as needed from the Resources tab. Your experience is in your hands!

![](./media/gs-leave-4.png)

> **Note:** If the VM is not in use, please **deallocate** it to avoid unnecessary resource consumption.

---

## Let's Get Started with Microsoft Azure

1. In the JumpVM, click on **Azure Portal** browser shortcut which is created on the desktop.

   ![](./media/gs-up1.png)

1. On the **Sign into Microsoft** tab, you will see the login screen. Enter the provided email or username and click **Next** to proceed.

   - Email/Username: <inject key="AzureAdUserEmail"></inject>

     ![](./media/gs-lab3-g2.png)

1. Now, enter the following Temporary Access Pass and click on **Sign in**.

   - Temporary Access Pass: <inject key="AzureAdUserPassword"></inject>

     ![](./media/gs-lab3-g3.png)

1. If you see the pop-up **Stay Signed in?**, click No.

   ![](./media/gs-4.png)

---

## Explore Your Dynamics 365 Environment

Your sandbox includes a pre-configured Dynamics 365 Sales Enterprise environment with seeded sample data. Before starting the challenge, familiarise yourself with what is available.

1. Open a new browser tab and navigate to Dynamics 365 Sales:

   ```
   https://make.powerapps.com
   ```

   Sign in with the provided credentials and select the pre-provisioned environment from the environment selector in the top-right corner.

1. Navigate to **Sales** > **Opportunities** and confirm that sample opportunity records are present. Your environment includes at minimum:

   - 20 closed-lost opportunity records across at least four distinct loss reason categories (pricing, competitor, delayed response, product fit, and long approval cycle).
   - Each opportunity record contains at minimum: account name, product line, deal value, close date, loss reason (field), owner, and activity history including notes and email logs.

   > **Note:** If the opportunity records are not visible, navigate to the Environment tab in your challenge portal to confirm you are connected to the correct Dynamics 365 environment. Contact CloudLabs support if the data is missing.

1. Open two or three opportunity records and review the **Timeline** section. The activity history - emails, notes, and call logs - is the raw content your extraction pipeline will process. Understanding this structure will help you design the export and indexing logic.

---

## Export Your Opportunity Dataset

Before starting Challenge 1, you need the raw data from Dynamics 365 available in Azure Blob Storage. This is the starting point for your extraction and indexing pipeline.

1. From Dynamics 365 Sales, navigate to **Opportunities** and filter the view to show only **Closed as Lost** records.

1. Export the opportunity records to a file format suitable for downstream processing. You may use the built-in Dynamics 365 export to Excel, Power Automate to extract and write to Blob Storage, or the Dynamics 365 Web API to programmatically export records with their associated notes and activities.

   Consider what fields and associated records you will need in your extraction pipeline:
   - Core opportunity fields: name, account, product line, deal value, expected close date, actual close date, loss reason (field value), owner
   - Associated notes and description text
   - Email subject lines and body text logged against the opportunity
   - Call log summaries

1. Upload or write the exported files to your Azure Blob Storage account. Organize them in a way that makes the source clear - for example, a folder per loss reason category or per account.

   > **Note:** You do not need to export every field. Focus on the content that a sales analyst would actually read to understand why a deal was lost.

---

## Verify Access to Required Services

Confirm you can access all services that will be used during the challenge before you begin:

1. From the Azure Portal, verify that the following resources are present in your assigned resource group:

   - Azure Storage Account
   - Azure AI Search instance
   - Azure AI Foundry workspace (or confirm you can create one)
   - Azure Document Intelligence resource

1. Open a new browser tab and navigate to Microsoft Copilot Studio:

   ```
   https://copilotstudio.microsoft.com
   ```

   Sign in with the provided credentials and confirm access is granted.

1. Open another browser tab and navigate to Power Automate:

   ```
   https://make.powerautomate.com
   ```

   Sign in and confirm access to Premium connectors and the Dynamics 365 connector are available under your license.

1. Navigate to Microsoft AI Foundry:

   ```
   https://ai.azure.com
   ```

   Sign in and confirm the pre-provisioned Foundry project is visible, or confirm you have permission to create a new project.

   > **Note:** If any service access is unavailable, navigate to the Environment tab in your challenge portal to retrieve alternate credentials or contact CloudLabs support.

---

Now, click on the **Next** from the lower right corner to move on to the challenge.

## Happy Hacking!!
