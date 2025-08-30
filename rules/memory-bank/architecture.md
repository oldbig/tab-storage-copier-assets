# System Architecture

The browser extension will have a simple, event-driven architecture based on the standard components of a Chrome Extension (Manifest V3).

## File Structure

The project will be organized as follows:

```
/
|-- manifest.json
|-- popup.html
|-- popup.css
|-- popup.js
|-- icons/
|   |-- icon16.png
|   |-- icon48.png
|   |-- icon128.png
```

## Key Components

1.  **`manifest.json`**
    *   **Role:** The core configuration file for the extension.
    *   **Responsibilities:** Defines permissions (`tabs`, `scripting`, `cookies`, `activeTab`), declares the popup page (`action`), and specifies icons.

2.  **`popup.html`**
    *   **Role:** The main user interface for the extension.
    *   **Responsibilities:** Contains the HTML structure for the source tab selector, data type checkboxes, cookie path input, submit button, and clear button. The target is implicitly the active tab.

3.  **`popup.css`**
    *   **Role:** Stylesheet for the popup UI.
    *   **Responsibilities:** Implements the visual design from the sketch, ensuring a clean and user-friendly interface.

4.  **`popup.js`**
    *   **Role:** The primary logic controller for the popup.
    *   **Responsibilities:**
        *   Populates the "source" tab dropdown, filtering out invalid or active tabs.
        *   Identifies the current active tab as the target.
        *   Handles all user interactions and validates inputs.
        *   Manages the `Submit` and `Clear` buttons' states.
        *   Displays success or error messages.
        *   Initiates the copy process by using the `chrome.scripting.executeScript` API.
        *   Implements the clear functionality with confirmation dialog.

## Data Flow for Copying Storage

1.  **User Action:** The user clicks the `Submit` button in `popup.html`.
2.  **Popup Logic (`popup.js`):**
    *   Identifies the selected source tab ID and gets the current active tab as the target.
    *   Validates that both tabs are eligible for the operation.
    *   Determines which data types (`localStorage`, `sessionStorage`, `cookies`) to copy.
3.  **Cookies:** If "Cookies" is checked, `popup.js` uses the `chrome.cookies` API to get cookies from the source tab and create them for the target tab's domain.
4.  **Local/Session Storage:**
    *   `popup.js` uses the `chrome.scripting.executeScript` API to execute code directly in the **source tab** to read data.
    *   The retrieved data is then passed to code executed in the **target (active) tab** to write the data.
5.  **Feedback:** `popup.js` updates the UI with a success or error message.

## Data Flow for Clearing Storage

1.  **User Action:** The user selects data types to clear and clicks the `Clear` button in `popup.html`.
2.  **Popup Logic (`popup.js`):**
    *   Shows a confirmation dialog with options to skip future confirmations.
    *   After confirmation, identifies the current active tab as the target.
    *   Validates that the target tab is eligible for the operation.
    *   Determines which data types (`localStorage`, `sessionStorage`, `cookies`) to clear.
3.  **Cookies:** If "Cookies" is checked, `popup.js` uses the `chrome.cookies` API to remove cookies from the target tab's domain.
4.  **Local/Session Storage:**
    *   `popup.js` uses the `chrome.scripting.executeScript` API to execute code directly in the **target (active) tab** to clear the selected storage types.
5.  **Feedback:** `popup.js` updates the UI with a success or error message.

## Profile Management Architecture

### Data Structure

The profile data will be stored in `chrome.storage.local` using the following structure:

```json
{
  "profiles": [
    {
      "name": "Profile A",
      "versions": [
        {
          "name": "Version 1",
          "timestamp": "ISO_8601_Date_String",
          "data": {
            "localStorage": { "key": "value" },
            "sessionStorage": { "key": "value" },
            "cookies": [ { "name": "cookieName", "value": "..." } ]
          }
        }
      ]
    }
  ]
}
```

### UI Components

The following UI elements will be added to support this feature:

*   "Save Profile" and "Load Profile" buttons.
*   Modal dialogs for the save and load workflows.