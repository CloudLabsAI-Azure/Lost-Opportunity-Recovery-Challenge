# Cleanup

## Deleting All Resources

In this task, you'll be deleting all resources created during the hackathon to ensure there are no lingering costs or orphaned configurations in the sandbox environment.

---

### Step 1: Remove the Copilot Studio Copilot

1. Navigate to Microsoft Copilot Studio:

   ```
   https://copilotstudio.microsoft.com
   ```

1. Select the copilot you created during the challenge.

1. Open **Settings** and scroll to the bottom to find the **Delete** option.

1. Confirm deletion. The copilot and all its topic flows will be permanently removed.

---

### Step 2: Delete Power Automate Flows

1. Navigate to Power Automate:

   ```
   https://make.powerautomate.com
   ```

1. Select **My Flows** from the left navigation.

1. Locate the flow you created to trigger on Dynamics 365 opportunity closure. Select the flow, click the three-dot menu, and choose **Delete**.

1. If you created any additional flows (for example, test flows or the optional Outlook calendar reminder flow), delete those as well.

---

### Step 3: Delete the AI Foundry Project

1. Navigate to Microsoft AI Foundry:

   ```
   https://ai.azure.com
   ```

1. Open the project you created during the challenge.

1. Navigate to **Settings** within the project and select **Delete project**.

1. Confirm the deletion. This removes the project configuration and any deployed agents associated with it.

---

### Step 4: Delete the Azure Resource Group

Deleting the resource group removes all Azure resources created during the challenge in a single operation, including Blob Storage, Azure AI Search, and Azure Document Intelligence.

1. Sign in to the Azure Portal:

   ```
   https://portal.azure.com
   ```

1. Navigate to **Resource groups** and select the resource group you used during the challenge.

1. Click **Delete resource group** at the top of the resource group blade.

1. Type the resource group name to confirm deletion and click **Delete**.

1. Wait for the deletion to complete. Verify that the resource group no longer appears in the Resource groups list.

---

### Step 5: Clean Up Dynamics 365 Test Data

1. Navigate to Dynamics 365 Sales:

   ```
   https://make.powerapps.com
   ```

1. Navigate to **Opportunities** and filter for any test opportunity records you created during the challenge (records you manually created, not the pre-seeded sample data).

1. Delete any test opportunity records, associated tasks, and any follow-up tasks that were auto-created by your Power Automate flow during testing.

1. If you created any test accounts or contacts during the challenge that are not part of the original pre-seeded dataset, delete those as well.

---

Congratulations on completing the Lost Opportunity Recovery Hackathon. You have successfully built an end-to-end AI-powered sales intelligence platform using Azure AI Search, Microsoft AI Foundry, Azure Document Intelligence, Dynamics 365, Power Automate, and Microsoft Copilot Studio.
