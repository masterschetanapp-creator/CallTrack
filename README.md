# CallTrack CRM - Cloud Setup Guide

This guide walks you through hosting your CallTrack CRM on **GitHub Pages** (free hosting) and connecting it to **Firebase Realtime Database** (free cloud database). Once configured, any user on any PC with internet access can log calls and view customer updates in real-time.

---

## Part 1: Create Your Firebase Cloud Database

1. Go to the [Firebase Console](https://console.firebase.google.com/) and log in with your Google account.
2. Click **Add project** (or **Create a project**).
   - Name your project: `calltrack-crm`
   - Click **Continue**.
   - Disable *Google Analytics* for this project (it simplifies the setup) and click **Create project**.
   - Wait a few seconds for the project to provision, then click **Continue**.

3. **Register a Web App** to get your configuration code:
   - On your project home screen, click the **Web icon (`</>`)** (looks like HTML brackets).
   - Enter an App nickname: `CallTrack CRM` (leave Firebase Hosting unchecked).
   - Click **Register app**.
   - You will see a code snippet. Look for the `firebaseConfig` object. It looks like this:
     ```javascript
     const firebaseConfig = {
       apiKey: "AIzaSy...",
       authDomain: "calltrack-crm-xxxx.firebaseapp.com",
       databaseURL: "https://calltrack-crm-xxxx-default-rtdb.firebaseio.com",
       projectId: "calltrack-crm-xxxx",
       storageBucket: "calltrack-crm-xxxx.appspot.com",
       messagingSenderId: "...",
       appId: "..."
     };
     ```
   - **Copy only the object part** (everything inside and including the curly braces `{ ... }`). You will paste this into the app later.

4. **Enable the Realtime Database**:
   - On the left sidebar menu, click **Build** -> **Realtime Database**.
   - Click **Create Database**.
   - Select your database location (default is fine) and click **Next**.
   - Choose **Start in test mode** (this enables public read/write access so the CRM can sync data without requiring user logins).
   - Click **Enable**.

---

## Part 2: Publish Your CRM to GitHub Pages

1. Log into your account on [GitHub](https://github.com/).
2. Click **New** (or **New repository**) to create a repository.
   - Repository name: `CallTrack`
   - Description: `Cloud CallTrack CRM`
   - Public/Private: Select **Public** (as selected during migration setup).
   - Click **Create repository**.

3. **Upload the files** from your local `CallTrack_Cloud` folder to this repository:
   - Click the link that says **"uploading an existing file"** in the Setup page.
   - Drag and drop all files from your `CallTrack_Cloud/` folder:
     - `index.html`
     - `sw.js`
     - `icon.svg`
     - `manifest.webmanifest`
     - `firebase-config.js`
   - Wait for the upload to complete, scroll down, and click **Commit changes**.

4. **Turn on GitHub Pages** to make it a website:
   - In your repository tabs, go to **Settings** (gear icon on the right).
   - Scroll down the left menu and click **Pages** (under Code and automation).
   - Under *Build and deployment* -> *Branch*:
     - Change from `None` to **`main`**.
     - Leave the folder as `/ (root)`.
     - Click **Save**.
   - Within 1-2 minutes, GitHub will publish your site. Refresh the page, and you will see a banner at the top of the settings page:
     > **Your site is live at `https://yourusername.github.io/CallTrack/`**

---

## Part 3: Connect the CRM to Your Database

1. Open your live website URL (from Step 4 above) in your web browser.
2. Go to the **Save / Load DB** tab (the last button in the navigation bar).
3. Under the **Firebase Cloud Database Sync** section, you will see a red indicator saying *Running Offline (Local Mode)*.
4. Click **Configure Firebase**.
5. Paste the config object you copied in **Part 1, Step 3** into the text field.
6. Click **Save & Connect**.
7. The status card will turn green and display **Cloud Connected**!

---

## Part 4: Migrate Your Existing Offline Data

1. On the **Save / Load DB** tab, scroll down to the **Load Database** section.
2. Click the folder upload zone (or drag and drop your file).
3. Select your existing database file from the shared drive: **`Y:\CallTrack_Offline_App\calltrack_db.json`**.
4. The CRM will read your records and automatically upload all customers and call history to your Firebase cloud database.
5. You will see a success message: *Loaded X customers & Y call logs!* 
6. Go back to the **Dashboard** or **Clients** tab to verify that all your data is present.

---

## How It Works Now

- **Automatic Real-time Sync**: When any user adds a client or logs a call, the changes are sent to Firebase immediately. Any other open browser screens will update instantly without refreshing.
- **Offline Fallback**: If internet connection is lost, the CRM continues to function and saves changes to the browser's local storage. Once reconnected, it syncs back to the cloud.
- **Independent Y:\\ Drive**: This cloud app runs independently of the old Y:\\ drive files. You can safely stop running the old server.
