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

Your sandbox includes a pre-configured Dynamics 365 Sales Enterprise environment. You will import the opportunity dataset in the next section.

1. Open a new browser tab and navigate to the Dynamics 365 home portal:

   ```
   https://home.dynamics.com
   ```

   Sign in with the provided credentials. The portal displays all Dynamics 365 apps available in your environment.

1. Locate **Sales Hub** in the app list and click it to open Dynamics 365 Sales.

   > **Note:** If Sales Hub is not listed, select **All Apps** or search for "Sales Hub" using the search bar at the top of the page. If it still does not appear, navigate to `https://make.powerapps.com`, select the environment from the top-right selector, go to **Apps**, and look for **Sales Hub** there.

1. Navigate to **Sales** > **Opportunities** and confirm the app loads correctly. The opportunities list will be empty at this point - you will import the dataset in the next section.

   > **Note:** If you cannot access the Opportunities view, confirm you are in the correct Dynamics 365 environment. Navigate to **Settings > Advanced Settings > Security > Users** and verify your user account has the **System Administrator** role assigned.

---

## Import the Opportunity Dataset

Your environment does not have pre-seeded sales data. Import the provided dataset to populate Dynamics 365 with 20 closed-lost opportunity records across five loss reason categories before starting Challenge 1.

1. Download the dataset file from the lab repository:

   ```
   https://raw.githubusercontent.com/CloudLabsAI-Azure/Lost-Opportunity-Recovery-Challenge/main/data/opportunities.csv
   ```

   Save the file to your Desktop or Downloads folder.

1. In Dynamics 365 Sales, select the **Settings** gear icon (top right) and navigate to **Advanced Settings**.

1. In the Business Management area, select **Data Management**, then select **Imports**.

1. Select **Import Data** and upload the `opportunities.csv` file you downloaded.

1. On the **Upload Data File** page, confirm the file type is detected as CSV and select **Next**.

1. On the **Select Data Map** page, select **Default (Automatic Mapping)** and select **Next**.

1. On the **Map Record Types** page, map the file to the **Opportunity** record type and select **Next**.

1. On the **Map Fields** page, confirm all four columns are mapped correctly:

   | CSV Column | Dynamics 365 Field |
   |---|---|
   | Topic | Topic |
   | Est. Revenue | Est. Revenue |
   | Actual Close Date | Actual Close Date |
   | Description | Description |

   All four fields should auto-map. Select **Next**.

1. On the **Review Settings and Import Data** page, select **Submit** to start the import. The import runs in the background and typically completes within 2-3 minutes.

1. Navigate to **Opportunities** and confirm 20 records are present. Filter by **Status Reason = Lost** to verify the closed-lost records are visible.

   > **Note:** If the import fails or record count is less than 20, re-download the CSV and retry. Ensure the file was not modified before upload. Contact CloudLabs support if the issue persists.

---

## Verify Access to Required Services

Confirm you can access all services that will be used during the challenge before you begin:

1. From the Azure Portal, verify that the following resources are present in your assigned resource group:

   - Azure Storage Account
   - Azure AI Search instance
   - Azure AI Foundry workspace (or confirm you can create one)

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