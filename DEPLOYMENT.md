# Deployment Guide: Hostinger & GitHub

This guide explains how to move your **CanGrants** app from AI Studio to your own Hostinger hosting and how to resolve GitHub authorization issues.

---

## 1. Deploying to Hostinger (Shared Hosting)

Since your app is a React Single Page Application (SPA), follow these steps to host it on Hostinger:

### Step A: Build the Project
1. In the AI Studio terminal (or locally if you download the code), run:
   ```bash
   npm run build
   ```
2. This creates a `dist/` folder containing all your production-ready files.

### Step B: Upload to Hostinger
1. Log in to your **Hostinger hPanel**.
2. Go to **Files** -> **File Manager**.
3. Navigate to the `public_html` folder (or your subfolder).
4. Upload the **contents** of the `dist/` folder directly into `public_html`.
5. **Crucial:** Ensure the `.htaccess` file (provided in this project) is also uploaded to the same folder. This file ensures that when you refresh the page, Hostinger knows to send the request back to `index.html` instead of showing a 404 error.

### Note on Subdirectories
If you are hosting the app in a subfolder (e.g., `yourdomain.com/cangrants/`), you must update the `base` property in `vite.config.ts` to `base: '/cangrants/'` before running `npm run build`.

### Step C: API Key Security Warning
On Hostinger Shared Hosting, your `GEMINI_API_KEY` will be bundled into the JavaScript files. 
*   **For a private portfolio/demo:** This is usually fine.
*   **For a public high-traffic site:** Anyone can find your key in the browser's "Network" tab. To prevent this, you would need a VPS or a backend proxy (Node.js/PHP).

---

## 2. Linking to GitHub (Detailed Instructions)

If you are having trouble with authorization, follow these exact steps:

### Step 1: Access the Export Menu
1. In the bottom-left corner of the AI Studio interface, click the **Settings** (gear icon).
2. Select **Export to GitHub**.

### Step 2: Handle the Authorization Popup
1. A new window will attempt to open to GitHub.com.
2. **Check your browser's address bar:** If you see a "Popup Blocked" icon, click it and select **"Always allow popups from ai.studio"**. This is the most common reason for authorization failure.
3. Sign in to your GitHub account if prompted.

### Step 3: Grant Permissions
1. GitHub will ask you to authorize **Google AI Studio**.
2. Click the green **Authorize** button.
3. If you want to export to an **Organization** repository, make sure to click "Request" or "Grant" next to the organization name on that same screen.

### Step 4: Create the Repository
1. Once authorized, return to AI Studio.
2. You will see a screen to "Create a new repository".
3. Enter a name (e.g., `can-grants-app`) and choose **Public** or **Private**.
4. Click **Export**.

### Troubleshooting Authorization
*   **"Permission Denied":** Ensure you are the owner of the GitHub account or have "Write" access to the organization you are trying to export to.
*   **Stuck Loading:** Refresh the AI Studio page and try again. Sometimes the OAuth token needs a fresh start.
*   **Wrong Account:** If you are logged into the wrong GitHub account in your browser, log out of GitHub.com first, then try the export again.

---

## 3. Environment Variables
Once the code is on GitHub or Hostinger, you must manually provide the API key.
*   **Local Development:** Create a `.env` file and add `VITE_GEMINI_API_KEY=your_key_here`.
*   **Hostinger:** Since it's a static build, Vite reads the key during `npm run build`. Make sure the environment variable is set in your build environment.
