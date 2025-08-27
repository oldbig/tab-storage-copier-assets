# Product Description

## 1. Problem Statement

Developers and testers frequently need to transfer session and storage data (local storage, session storage, cookies) between different browser tabs. This is often required for testing different user scenarios, debugging issues, or setting up a specific application state. The manual process of copying this data is tedious, error-prone, and time-consuming, hindering development and testing workflows.

## 2. Solution

This browser extension provides a simple and efficient solution to automate the transfer of storage and session data. It offers a user-friendly interface to copy data from a selected source tab to the **current active tab**.

## 3. User Experience and Functionality

The user interacts with the extension through a popup window. The workflow is as follows:

1.  **Open Popup:** The user clicks on the extension's icon in the browser toolbar to open the main interface.

2.  **Select Source Tab:**
    *   A dropdown list is populated with all eligible source tabs (excluding protected browser pages and the current active tab).
    *   The user selects the tab from which to copy the data. The target is always the current active tab.

3.  **Select Data to Copy:** The user can choose one or more of the following data types via checkboxes:
    *   `local storage`
    *   `session storage`
    *   `Cookies`

4.  **Configure Cookie Path (Optional):**
    *   If the `Cookies` checkbox is selected, an input field (`Cookie path save to`) becomes editable.
    *   The user can specify a new path (e.g., `/`) for the copied cookies.
    *   If the input is left empty, the cookies will retain their original path from the source tab.
    *   A help icon (`?`) next to the input will provide a brief explanation of the path setting rules on hover or click.

5.  **Submit Action:**
    *   The `Submit` button is enabled only when a source tab and at least one data type checkbox are selected.
    *   Clicking `Submit` initiates the copy process. Before copying, the extension validates that both the source and the current active tab are valid (e.g., not frozen or a protected page).
    *   The extension then reads the selected data from the source tab's context and writes it to the current active tab's context.

6.  **View Feedback:**
    *   After the operation is complete, a status message appears below the `Submit` button.
    *   The message will confirm the success of the operation or provide a clear error message if validation fails (e.g., "Source tab is frozen" or "Current tab is not a valid target").
    *   This message persists until the user modifies any of the inputs (tab selection, checkboxes, or cookie path), at which point it disappears to prepare for the next operation.

7.  **Quick Clear Tool:**
    *   The user can select one or more data types to clear from the current active tab using the checkboxes.
    *   The `Clear` button (positioned alongside the `Submit` button) is enabled only when at least one data type checkbox is selected.
    *   Clicking `Clear` presents a confirmation dialog with two options:
        - "Always accept" - Skip confirmation for all future clear operations
        - "Accept only for current domain and port" - Skip confirmation only for the current domain and port
    *   After confirmation (or if confirmation is skipped based on user preferences), the extension clears the selected data types from the current active tab.
    *   A status message appears below the buttons showing the result of the clear operation, confirming success or providing error details.