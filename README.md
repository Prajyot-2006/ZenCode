# ⚡ LeetBoost

> A Chrome extension that enhances LeetCode with themed submission
> celebrations, submission analytics, and AI-powered Time & Space
> Complexity analysis.

## 🚀 Overview

**LeetBoost** is a Manifest V3 Chrome extension built with React and
Vite.

It provides:

-   🎮 GTA San Andreas and Minecraft celebration themes
-   🔊 Passed/failed submission sounds
-   📊 Submission analytics
-   🤖 Gemini-powered Time & Space Complexity analysis
-   💾 Per-problem complexity caching
-   ⚡ Network-based LeetCode submission detection

The project uses a React/Vite Chrome extension frontend and a
Node.js/Express backend for Gemini analysis.

## ✨ Features

### 🎮 Submission Celebrations

LeetBoost detects LeetCode submission results and displays a themed
celebration.

Supported themes:

-   GTA San Andreas
-   Minecraft

Each theme contains:

``` text
passed.png
passed.mp3
failed.png
failed.mp3
```

The celebration supports:

-   Passed/failed image
-   Sound effects
-   Attempt information
-   Animated popup
-   Sound ON/OFF
-   Celebration ON/OFF
-   Theme selection

### 📊 Submission Analytics

LeetBoost tracks:

-   Problems attempted
-   Accepted submissions
-   Failed submissions
-   Attempts for a problem
-   Total solving time
-   Current streak
-   Paste events
-   Bulk paste events
-   Tab switches

### 🤖 AI Complexity Analyzer

LeetBoost uses the Gemini API to analyze submitted LeetCode code.

It returns:

``` text
Time Complexity
Space Complexity
```

Example:

``` text
🤖 Time Complexity : O(N)
💾 Space Complexity : O(N)
```

The backend keeps the Gemini API key private through an environment
variable.

``` text
Chrome Extension
       ↓
LeetBoost Backend
       ↓
Gemini API
       ↓
Time + Space Complexity
       ↓
Chrome Extension
```

### 💾 Complexity Caching

A successful complexity analysis is saved per LeetCode problem slug
using:

``` text
chrome.storage.local
```

For example:

``` text
two-sum
```

First successful analysis:

``` text
LeetCode
   ↓
Backend
   ↓
Gemini
   ↓
Save result
```

Later submissions of the same problem:

``` text
LeetCode
   ↓
Saved complexity result
```

No new backend/Gemini analysis is needed after a successful result has
been cached.

If an analysis request fails, no result is cached, allowing a later
retry.

## 🧠 Submission Detection

LeetBoost intercepts LeetCode's submission request instead of relying
only on visible page text.

It obtains the submission ID and retrieves the submitted code through
LeetCode's GraphQL submission details endpoint.

The extension uses the submission status to distinguish accepted and
failed submissions.

## 🏗️ Project Structure

``` text
LeetBoost/
├── public/
│   ├── assets/
│   │   ├── gta/
│   │   │   ├── failed.mp3
│   │   │   ├── failed.png
│   │   │   ├── passed.mp3
│   │   │   └── passed.png
│   │   └── minecraft/
│   │       ├── failed.mp3
│   │       ├── failed.png
│   │       ├── passed.mp3
│   │       └── passed.png
│   ├── content.js
│   ├── icon128.png
│   ├── manifest.json
│   └── pageInterceptor.js
│
├── src/
│   ├── components/
│   │   ├── AnalyticsView.jsx
│   │   ├── FloatingButton.jsx
│   │   ├── Header.jsx
│   │   ├── ReportView.jsx
│   │   └── SettingsView.jsx
│   ├── utils/
│   │   ├── celebration.js
│   │   ├── complexity.js
│   │   ├── interceptor.js
│   │   ├── settings.js
│   │   ├── tracker.js
│   │   └── user.js
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
│
├── server/
│   ├── server.js
│   ├── package.json
│   └── .env
│
├── .gitignore
├── package.json
├── vite.config.js
└── README.md
```

## 🛠️ Tech Stack

### Extension

-   React
-   Vite
-   JavaScript
-   Tailwind CSS
-   Chrome Extension Manifest V3
-   Chrome Storage API

### Backend

-   Node.js
-   Express.js
-   CORS
-   dotenv
-   Google Gemini API

### Deployment

-   Render for the backend

## ⚙️ Local Setup

### 1. Clone

``` bash
git clone https://github.com/Prajyot-2006/LeetBoost.git
cd LeetBoost
```

### 2. Install frontend dependencies

``` bash
npm install
```

### 3. Install backend dependencies

``` bash
cd server
npm install
```

### 4. Configure Gemini

Create:

``` text
server/.env
```

Add:

``` env
GEMINI_API_KEY=YOUR_GEMINI_API_KEY
```

Never commit the API key.

## ▶️ Run the Backend

From the `server` directory:

``` bash
npm start
```

The backend exposes:

``` text
POST /analyze-complexity
```

## 🔨 Build the Extension

From the project root:

``` bash
npm run build
```

The production extension is generated in:

``` text
dist/
```

## 🌐 Load in Chrome

1.  Open `chrome://extensions`
2.  Enable **Developer mode**
3.  Click **Load unpacked**
4.  Select the project's `dist/` folder
5.  Open LeetCode
6.  Submit a problem

## 🔐 Security

The Gemini API key is kept on the backend and is not placed inside the
extension.

``` text
LeetBoost
    │
    │ submitted code
    ▼
Backend
    │
    │ private API key
    ▼
Gemini
```

The API key should remain in:

``` text
server/.env
```

and should never be committed to GitHub.

## 🔄 Submission Flow

``` text
User submits solution
        ↓
LeetCode /check/ request
        ↓
LeetBoost intercepts result
        ↓
Get submission ID
        ↓
Fetch submitted code
        ↓
Detect result
        ↓
┌───────┴────────┐
↓                ↓
Celebration   Complexity
                 ↓
            Check cache
                 ↓
        ┌────────┴────────┐
        ↓                 ↓
     Cached          Not cached
        ↓                 ↓
    Show result     Backend → Gemini
                         ↓
                    Save result
```

## 📌 AI Analysis Rules

The backend asks Gemini to return JSON in this format:

``` json
{
  "timeComplexity": "O(N)",
  "spaceComplexity": "O(N)"
}
```

The analyzer reports **total space complexity**, including output data
structures.

## 🎨 Themes

### GTA San Andreas

``` text
public/assets/gta/
```

### Minecraft

``` text
public/assets/minecraft/
```

Each theme contains passed and failed images and audio.

## 🧪 Development

After making changes:

``` bash
npm run build
```

Then reload the extension from:

``` text
chrome://extensions
```

## ⚠️ Notes

-   LeetBoost is designed for the LeetCode website.
-   LeetCode internal API/page changes may require updates.
-   Gemini availability and rate limits depend on the configured Google
    AI Studio/Gemini account and model.
-   Never expose the Gemini API key publicly.

## 🎯 Future Improvements

-   More celebration themes
-   More detailed analytics
-   Additional AI analysis features
-   Further animation synchronization improvements
-   More customization options
-   Chrome Web Store release
-   Additional coding-platform support

## 👨‍💻 Author

**Prajyot**

Built as a personal project to make LeetCode practice more interactive
while learning Chrome Extensions, React, backend development, APIs, and
AI integration.

------------------------------------------------------------------------

## ⭐ LeetBoost

**Solve. Submit. Get feedback. Keep improving.**
