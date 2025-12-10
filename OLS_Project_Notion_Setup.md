# Overlake Square Framing Project System Setup

This document outlines how to set up a comprehensive **project workspace** in Notion using ChatGPT’s Notion MCP connector.  It defines the databases and views needed to manage tasks, schedules, deliveries, inventory and meetings for the **Overlake Square Framing** project.  The goal is to build a single dashboard that pulls together multiple filtered views so you can track critical items at a glance.

> **Note:**  These instructions assume you have already connected ChatGPT to your Notion workspace via the **Notion MCP connector**.  The Notion MCP connector (in ChatGPT → Settings → Apps → Connectors → Notion) allows ChatGPT to create and update pages and databases in your workspace without manually generating an API token.

## 1. Base prompt for ChatGPT
Copy and paste the following prompt into ChatGPT after the Notion connector is connected.  The agent will automatically invoke the required MCP tools (e.g. `create pages`, `create database`) to build the structure.  Adjust names or fields if your project needs different terminology.

```text
Create a top-level Notion page called **“Overlake Square Framing Project”**.  Under this page:

1. Create a database called **“Project Dashboard”** with the following properties:
   - **Name** (title)
   - **Status** (select: Not Started, In Progress, Done, On Hold)
   - **Due Date** (date)
   - **Priority** (select: Low, Medium, High, Critical)
   - **Category** (multi‑select: Hot List, Schedule, Open Items, Deliveries, Docs, Inventory)
   - **Owner** (person)
   - **Description** (text)

2. Add views to **Project Dashboard**:
   - **Dashboard Table** – default table view showing all tasks.
   - **Calendar** – calendar view using **Due Date**.
   - **Hot List** – table filtered to tasks where **Priority = High** and **Due Date** is within the next 7 days.
   - **Schedule** – timeline (Gantt) view grouped by **Status**, sorted by **Due Date**.
   - **3‑Week Look Ahead** – table filtered to tasks with **Due Date** in the next 21 days.
   - **Open Items** – table filtered to tasks where **Status ≠ Done**.

3. Create separate databases:
   - **Deliveries**
     - **Item** (title)
     - **Expected Date** (date)
     - **Status** (select: Pending, Shipped, Delivered)
     - **Supplier** (text)
     - **Notes** (text)

   - **Documents Hub**
     - **Document Name** (title)
     - **Type** (multi‑select: Design, Contract, Permit, RFI, Submittal, Inspection)
     - **Link** (url or file)
     - **Date Added** (date)

   - **Inventory – Lumber Stock**
     - **Material** (title)
     - **Quantity** (number)
     - **Location** (text)
     - **Reorder Level** (number)

4. Create a page called **“Weekly Meeting”**.  On this page, insert linked views of **Project Dashboard**, **Deliveries** and **Inventory**.  Configure each linked view:
   - **Upcoming Tasks** – from Project Dashboard, filter to tasks with **Due Date** within the next 7 days and **Status ≠ Done**.
   - **Overdue Items** – from Project Dashboard, filter to tasks where **Due Date < today** and **Status ≠ Done**.
   - **Upcoming Deliveries** – from Deliveries, filter to entries where **Expected Date** within the next 14 days.
   - **Low Stock Alert** – from Inventory, filter to materials where **Quantity ≤ Reorder Level**.

5. Optional:  After the databases are created, link tasks or deliveries together using relation properties.  For example, add a **Related Delivery** relation in the Project Dashboard to reference a row in **Deliveries**.

```

### How it works
- ChatGPT will create each database with the specified properties using the `create database` MCP tool.
- For each database, ChatGPT will create views (`create database view`) with filters and sorts as described.
- The **Weekly Meeting** page uses linked views to assemble a live agenda that pulls data from your databases.
- The **Category** property on the main dashboard helps you slice tasks into Hot List, Schedule, Deliveries, Docs and Inventory categories.

## 2. Preparing your data
If you have existing schedules, inventory lists or delivery logs in Excel/CSV files or other documents, import them into Notion before running the prompt.  You can import a CSV directly into the appropriate database (e.g. import your deliveries spreadsheet into the **Deliveries** database) or copy the rows into the new database after it is created.

Because the connectors in this environment are **read‑only**, they cannot upload files or push changes directly to Notion or GitHub.  To populate the new databases with data from your files:
1. Open your source files (e.g. Structural Schedule.xlsx, Summary‑RoughFrame.xlsx) on your computer.
2. In Notion, open the newly created database (Deliveries, Inventory, etc.) and choose **“New → Import”** to import the CSV/spreadsheet or manually copy/paste the data.

## 3. Updating GitHub
This repository currently only contains a README.  Since direct write operations via the connector are not available, you must update the repository manually:
1. Save this file (`OLS_Project_Notion_Setup.md`) to your local clone of **OLS‑Rough_Frame**.
2. Commit and push the file:
   ```sh
   git add OLS_Project_Notion_Setup.md
   git commit -m "Add Notion project setup instructions"
   git push origin main
   ```
3. Share the repository link with collaborators so they can follow the setup process.

## 4. Next steps
- **Run the prompt** in ChatGPT to have it build the workspace automatically.
- **Import your data** into each database as needed.
- **Use the Weekly Meeting page** to track tasks, deliveries and inventory each week.

This file is provided as a starting point; feel free to modify fields or views to match your project’s workflow.
