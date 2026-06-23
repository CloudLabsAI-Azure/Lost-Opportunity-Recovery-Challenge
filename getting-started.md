## Getting Started with Challenge

We've prepared a seamless environment for you to explore and learn. Let's begin by making the most of this experience.

### Accessing Your Challenge Environment

Once you're ready to dive in, your virtual machine and challenge guide will be right at your fingertips within your web browser.

![](./media/know-ent-gs-g12.png)

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

## Let's Get Started with Microsoft Azure

1. In the JumpVM, click on **Azure Portal** browser shortcut which is created on the desktop.

   ![](./media/know-ent-gs-g11.png)

1. On the **Sign into Microsoft** tab, you will see the login screen. Enter the provided email or username and click **Next** to proceed.

   - Email/Username: <inject key="AzureAdUserEmail"></inject>

     ![](./media/gs-lab3-g2.png)

1. Now, enter the following Temporary Access Pass and click on **Sign in**.

   - Temporary Access Pass: <inject key="AzureAdUserPassword"></inject>

     ![](./media/gs-lab3-g3.png)

1. If you see the pop-up **Stay Signed in?**, click No.

   ![](./media/gs-4.png)

---

## Download the Opportunity Dataset

Two data files have been prepared for this lab. Download the zip archive now — it contains both files you will need across the challenges.

1. Download the dataset archive:

   ```
   https://github.com/CloudLabsAI-Azure/hack-in-a-day-data/archive/refs/heads/lost-dropped-opportunities.zip
   ```

1. Once downloaded, extract the zip. Inside you will find two files:
   - **`opportunities.csv`** — 20 closed-lost opportunity records across five loss-reason categories (Pricing, Competitor, Product Fit, Long Approval Cycle, Delayed Response). You will upload this to Azure Blob Storage in Challenge 1 and import it into Dynamics 365 in Challenge 2.
   - **`accounts.csv`** — the 20 company (Account) records that correspond to the opportunities. You will import this into Dynamics 365 in Challenge 2 **before** importing the opportunities file, so that the Potential Customer lookup field resolves correctly.

   Save both extracted files to your Desktop or Downloads folder.

---

## Verify Access to Required Services

Confirm you can access all services that will be used during the challenge before you begin:

1. Open a new browser tab and navigate to Microsoft Copilot Studio:

   ```
   https://copilotstudio.microsoft.com
   ```

   Sign in with the provided credentials and confirm access is granted.

1. Open another browser tab and navigate to Power Automate:

   ```
   https://make.powerautomate.com
   ```

   Sign in and confirm that access to Premium connectors and the Dynamics 365 connector is available under your license.

1. Navigate to Microsoft AI Foundry:

   ```
   https://ai.azure.com
   ```

   Sign in and confirm the pre-provisioned Foundry project is visible, or confirm you have permission to create a new project.

   > **Note:** If any service access is unavailable, navigate to the Environment tab in your challenge portal to retrieve alternate credentials or contact CloudLabs support.

---

Now, click on **Next** from the lower right corner to move on to the challenge.

## Happy Hacking!!
