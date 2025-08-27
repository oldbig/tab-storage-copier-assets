# Technology Stack

This project is a browser extension and will be built using standard web technologies. The core components are:

-   **HTML:** For the structure of the popup interface.
-   **CSS:** For styling the popup interface to match the design.
-   **JavaScript (ES6+):** For the extension's logic, including UI interactions and communication with browser APIs.

## Browser Extension APIs

We will rely on the Chrome Extension APIs (Manifest V3) to interact with the browser. Key APIs include:

-   **`manifest.json`:** The core configuration file for the extension, defining its properties, permissions, and components.
-   **`chrome.tabs`:** To get a list of all open tabs and their details (ID, title, URL) to populate the source and target dropdowns.
-   **`chrome.scripting`:** To execute scripts directly in the context of the source and target tabs. This is essential for accessing `localStorage` and `sessionStorage`, which are isolated to specific pages.
-   **`chrome.cookies`:** To get, set, and manage cookies for specific domains.
-   **`chrome.storage.local`:** Potentially for storing user preferences or extension state, although not a primary requirement initially.

## Development and Build

-   No complex build tools (like Webpack or Vite) are planned at this stage to keep the setup simple. Development will be done with plain HTML, CSS, and JavaScript.
-   The extension will be designed for **Manifest V3**, the current standard for Chrome extensions.