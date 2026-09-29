# Technical Assessment Submission: Bug Reporting & n8n Automation

## Overview
This repository contains the complete deliverables for the technical assessment, comprising bug analysis for a web application and an automated background workflow built using **n8n**.

---

## Task 1: Bug Investigation & Root-Cause Analysis

### Target Application
* **URL:** `https://demo.realworld.show`.

### Identified Bugs & Root Causes
1. **Password Policy Flaws:** 
   * *Issue:* The registration system permits weak passwords (such as single-character inputs) without proper validation constraints.
   * *Root Cause:* Insufficient client-side and server-side regular expression checks or length validation policies on the input fields.
2. **Lack of Visual Loading States:**
   * *Issue:* When submitting forms or navigating data, the interface provides no visual feedback (such as spinners or disabled buttons), leading to repeated submissions.
   * *Root Cause:* Asynchronous actions lack UI state management handlers to reflect loading or pending request states.
3. **Authentication Persistence Issues:**
   * *Issue:* Tokens or session states improperly persist or fail to clear cleanly upon logging out, causing potential state leakage.
   * *Root Cause:* Incomplete local/session storage cleanup or missing authorization header invalidation logic in the client session lifecycle.

---

## Task 2: Automated API Workflow & Notification System

### Workflow Architecture
The n8n workflow executes the following sequence:
1. **Schedule Trigger:** Automatically initiates the pipeline on a timed schedule.
2. **HTTP Request 1 (Main API):** Fetches the primary dataset from `https://jsonplaceholder.typicode.com/posts`.
3. **Code Node:** Executes custom JavaScript to slice and limit the dataset (`$input.all().slice(0, 5)`).
4. **HTTP Request 2 (Enrichment API):** Queries `https://jsonplaceholder.typicode.com/users/` to fetch record metadata.
5. **IF Node:** Filters items based on specific attribute criteria (e.g., matching ID `2`).
   * **True Branch:** Routes matching items forward.
   * **False Branch:** Cleanly terminates non-matching items.
6. **Gmail Node:** Formats and dispatches a customized email digest (`name`, `email`, `id`) to the designated recipient.

### Error Handling
* **"Continue Workflow"** (`continueRegularOutput`) is enabled under the *On Error* configuration for both HTTP Request nodes. This ensures network drops or API failures are handled gracefully without crashing the entire workflow.

### Task 2 Deliverables
* **`Task2_Workflow_OmkarParade.json`**: The exported n8n workflow file.
* **Canvas Screenshot**: Visual representation of the connected node pipeline.
* **Execution Screenshot**: Live proof of data passing through the output data panel.
