# 插件发布与部署指南

本文档旨在说明如何将“Tab Storage Copier”插件发布到 Chrome 网上应用店、Microsoft Edge 加载项和 Firefox Browser ADD-ONS，内容涵盖手动发布与自动化 (CI/CD) 部署两种流程。

---

## 1. 手动发布流程

首次提交插件时，必须手动操作。

### 1.1. 通用准备步骤

在上传之前，请准备好以下材料：

1.  **代码包 (`.zip`)**: 创建一个仅包含必要插件文件的 zip 压缩包。
2.  **图标**: 确保您拥有 `16x16`, `48x48`, `128x128` 像素的图标。
3.  **宣传材料**: 截图、简短描述、详细描述。
4.  **隐私政策链接**: 准备一个公开可访问的隐私政策页面。这对通过所有商店的审核至关重要。
    - **当前链接**: `https://gist.github.com/oldbig/93f32b2b8e862b1adb2a554e49ed3552`

### 1.2. 发布到 Chrome 网上应用店

1.  **注册**: 在 [Chrome 开发者信息中心](https://chrome.google.com/webstore/developer/dashboard) 支付 5 美元的一次性注册费。
2.  **上传**: 点击“+ 新建商品”，并上传您的 `.zip` 包。
3.  **填写详情**:
    *   **商店列表**: 填写标题、描述，上传宣传材料。
    *   **隐私权惯例**: 准确声明权限原因，并**提供隐私政策链接**。
4.  **提交**: 点击“提交以供审核”。

### 1.3. 发布到 Microsoft Edge 加载项

1.  **注册**: 在 [Microsoft 合作伙伴中心](https://partner.microsoft.com/en-us/dashboard/microsoftedge) 注册免费开发者账户。
2.  **创建提交**: 点击“创建新扩展”。
3.  **上传**: 上传同一个 `.zip` 包。
4.  **填写详情**: 填写商店列表信息，**提供隐私政策 URL**。
5.  **提交**: 点击“发布”以提交进行认证。

### 1.4. 发布到 Firefox Browser ADD-ONS

发布到 Firefox 时需特别注意兼容性。

1.  **注册**: 在 [Firefox Add-on Developer Hub](https://addons.mozilla.org/en-US/developers/) 创建账户。
2.  **上传**: 点击 "Submit a New Add-on" 并上传 `.zip` 包。
3.  **兼容性检查**:
    *   确保 `manifest.json` 中的 `browser_specific_settings.gecko.strict_min_version` 设置为 `109.0` 或更高，以保证 MV3 API 的兼容性。
4.  **填写详情**: 填写列表信息，并**务必在“隐私政策”字段中提供您的隐私政策链接**。
5.  **提交**: 提交审核。

---

## 2. 自动化部署 (CI/CD)

使用 GitHub Actions 实现自动化更新。

### 2.1. 通用工作流程

1.  **获取 API 凭据**: 从各应用商店的开发者后台获取。
2.  **存储凭据**: 安全地存储在 GitHub Secrets 中。
3.  **创建工作流文件**: 在 `.github/workflows/deploy.yml` 中定义流水线。

### 2.2. 针对 Chrome 网上应用店的自动化

1.  **API 设置**:
    *   在 Google Cloud Console 中，启用 "Chrome Web Store API"。
    *   创建 OAuth 2.0 客户端 ID，并生成 Refresh Token。
2.  **GitHub Secrets**:
    *   `CHROME_CLIENT_ID`, `CHROME_CLIENT_SECRET`, `CHROME_REFRESH_TOKEN`, `CHROME_EXTENSION_ID`
3.  **GitHub Actions 工作流 (`deploy.yml`)**:
    ```yaml
    - name: Upload to Chrome Web Store
      uses: w3c-actions/chrome-extension-upload@v1
      with:
        # ... secrets ...
        zip-file: extension.zip
    ```

### 2.3. 针对 Microsoft Edge 加载项的自动化

1.  **API 设置**:
    *   在 Microsoft 合作伙伴中心找到 `Product ID`。
    *   创建 AAD 应用以获取 `Tenant ID`, `Client ID`, `Client Secret`。
2.  **GitHub Secrets**:
    *   `EDGE_PRODUCT_ID`, `EDGE_CLIENT_ID`, `EDGE_CLIENT_SECRET`, `EDGE_TENANT_ID`
3.  **GitHub Actions 工作流 (`deploy.yml`)**:
    ```yaml
    - name: Upload to Edge Add-ons Store
      uses: wext-shipit/edge-extension-uploader@v1
      with:
        # ... secrets ...
        zip-path: extension.zip
    ```

### 2.4. 触发部署

部署工作流由推送 Git 标签触发：
```bash
# 1. 在 manifest.json 中更新版本号
# 2. 提交并推送更改
git commit -am "发布 v1.1.0"
git push

# 3. 创建并推送新标签
git tag v1.1.0
git push origin v1.1.0
```

---

## 3. 商店文案与隐私说明 (复制粘贴用)

### 3.1. 插件名称

Tab Storage Copier

### 3.2. 简短描述 (Short Description, 132 字符以内)

**中文版:**
一键轻松复制 Local Storage、Session Storage 和 Cookies，显著提升开发与测试效率。

**英文版:**
Easily copy Local Storage, Session Storage, and Cookies between tabs with one click to boost development and testing efficiency.

### 3.3. 详细描述 (Detailed Description)

**中文版:**

**问题**
作为开发者和测试人员，我们经常需要在不同的浏览器标签页之间转移 Local Storage、Session Storage 和 Cookies，以模拟不同的用户场景或调试特定问题。手动操作这个过程不仅繁琐、耗时，而且极易出错，严重影响了工作效率。

**解决方案**
Tab Storage Copier 提供了一个简洁高效的解决方案。通过一个清爽的用户界面，您可以轻松完成以下操作：
*   **选择来源与目标**: 从下拉列表中清晰地选择需要复制数据的来源标签页和目标标签页。
*   **灵活选择数据**: 通过复选框自由组合需要复制的数据类型：`Local Storage`、`Session Storage` 或 `Cookies`。
*   **自定义 Cookie 路径**: 在复制 Cookie 时，您可以选择性地为其指定一个新的生效路径（例如 `/`），以适应不同的测试环境。
*   **即时反馈**: 操作完成后，插件会立即显示成功或失败的状态消息，让您对结果一目了然。

本插件旨在成为您开发工具箱中一个轻量、可靠且不可或缺的效率工具。

**英文版:**

**The Problem**
As developers and testers, we frequently need to transfer Local Storage, Session Storage, and Cookies between different browser tabs to simulate various user scenarios or debug specific issues. Manually performing this process is tedious, time-consuming, and highly prone to errors, significantly hindering our workflow.

**The Solution**
Tab Storage Copier offers a clean and efficient solution to this problem. Through a streamlined user interface, you can easily:
*   **Select Source and Target**: Clearly pick the source and target tabs for the data transfer from dropdown lists.
*   **Choose Data Flexibly**: Freely select any combination of data types to copy: `Local Storage`, `Session Storage`, or `Cookies`.
*   **Customize Cookie Path**: When copying cookies, you can optionally specify a new path (e.g., `/`) to fit different testing environments.
*   **Get Instant Feedback**: The extension provides immediate success or error messages after each operation, ensuring you always know the result.

This extension is designed to be a lightweight, reliable, and indispensable productivity tool in your development toolkit.

### 3.4. 隐私权惯例 - 权限使用说明 (Justification for Permissions)

**核心思想**: 本插件不收集、不存储、不传输任何用户数据。所有操作均在本地浏览器中完成。

**权限申请理由 (英文，供商店审核用):**

*   **`tabs`**: This permission is required to list all open tabs in the "source" and "target" dropdown menus, allowing the user to select which tabs to work with. The extension only reads tab titles and IDs for display purposes.
*   **`scripting`**: This permission is essential for accessing and transferring storage data. The extension programmatically injects a script into the user-selected source tab to read its `localStorage` and `sessionStorage`, and into the target tab to write this data. This is the only way to access page-specific storage, and the script runs only when the user initiates a copy action.
*   **`cookies`**: This permission is needed to read cookies from the source tab's domain and write them to the target tab's domain, as requested by the user. The extension does not store or transmit any cookie data externally.

**隐私政策总结 (中文，如果需要单独的隐私政策页面):**
本插件的所有功能均在您的本地浏览器中运行，不会收集、记录或将您的任何数据（包括标签页信息、存储数据或 Cookies）发送到任何远程服务器。您的隐私是绝对安全的。