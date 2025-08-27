# Stop Wasting Time in DevTools: I Built a Tool to Sync Browser Storage in One Click

As developers, we live in browser DevTools. But let's be honest, some tasks are just plain tedious. For me, that task was synchronizing `localStorage`, `sessionStorage`, and `cookies` between tabs.

Testing an app with different user roles (admin, guest, etc.) meant a constant, frustrating cycle: log out, clear storage, log back in. Debugging a state-specific bug was a nightmare of manually copying and pasting key-value pairs. It was slow, error-prone, and a major drag on my productivity.

After complaining about it one too many times, I decided to fix it. I built **Tab Storage Copier**, a simple browser extension to automate this entire process.

**Get it here, it's free:**
*   [Chrome Web Store](https://chromewebstore.google.com/detail/tab-storage-copier/gnbnohblpgfdellglplgpnfjpokalgen?hl=en-US&utm_source=ext_sidebar)
*   [Microsoft Edge Add-ons](https://microsoftedge.microsoft.com/addons/detail/tab-storage-copier/hfcopmhdfklplklpmpknbmaakjemeadc)
*   [Firefox Browser ADD-ONS](https://addons.mozilla.org/en-US/firefox/addon/tab-storage-copier/)

---

### How It Works

The entire workflow is designed to be fast and intuitive.

#### 1. Select Source & Data

Just open the extension, pick your source tab from the dropdown, check the data you want to copy (`localStorage`, `sessionStorage`, `cookies`), and click the "Copy to Current Tab" button. That's it. The data is instantly transferred to your active tab.

![Select source and data types](https://raw.githubusercontent.com/oldbig/tab-storage-copier-assets/main/pictures/chrome-1.png)

#### 2. Customize Cookie Paths

When you're copying cookies, you can even set a new path (like `/`) for the destination. This is a small feature that's incredibly useful for certain testing scenarios.

![Customize cookie path](https://raw.githubusercontent.com/oldbig/tab-storage-copier-assets/main/pictures/chrome-2.png)

#### 3. Get Instant Feedback

The extension gives you immediate feedback, so you know right away if the operation succeeded or if something went wrong. No guesswork involved.

![Get success feedback](https://raw.githubusercontent.com/oldbig/tab-storage-copier-assets/main/pictures/chrome-3.png)

#### 4. Bonus: Quick Clear Tool

I also added a "Clear" button. Now you can wipe the current tab's storage and cookies with a single click, making it effortless to reset your application's state.

![Use the Quick Clear tool](https://raw.githubusercontent.com/oldbig/tab-storage-copier-assets/main/pictures/chrome-4.png)

---

### Why You Should Try It

*   **Save Time & Effort**: Stop the tedious manual work and focus on what matters: building and debugging.
*   **Simplify Your Workflow**: Testing different user accounts and application states becomes trivial.
*   **Eliminate Errors**: Avoid mistakes from manually copying and pasting data.

This tool was born from a real-world frustration, and it has already become an indispensable part of my own development toolkit. It's built with zero dependencies, using standard `chrome.*` APIs for a lightweight and secure experience.

I hope it can save you as much time as it has saved me. Give it a try and let me know what you think in the comments! All feedback and feature suggestions are welcome.