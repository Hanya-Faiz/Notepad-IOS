# Prompt Master Notes — Setup & Deployment Guide

## 🚀 Quick Start (Local Development)

### Step 1: Clone the Repository
```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
```

### Step 2: Install Dependencies
```bash
# Using bun (recommended)
bun install

# Or using npm
npm install
```

### Step 3: Set Up Environment Variables
```bash
# Copy the example file
cp .env.example .env.local

# Open .env.local in your editor and fill in your Google credentials
```

### Step 4: Run the App
```bash
# Using bun
bun run dev

# Using npm
npm run dev
```

Open http://localhost:5173 in your browser.

---

## 🔑 Google Drive API Setup (for Cloud Backup)

The app works perfectly without Google Drive — all notes are saved to localStorage automatically. Google Drive is only needed for the cloud backup feature.

### Step 1: Create a Google Cloud Project
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click **"New Project"** → Give it a name (e.g., "Prompt Master Notes") → Click **Create**
3. Make sure your new project is selected in the top dropdown

### Step 2: Enable Google Drive API
1. Go to **APIs & Services → Library**
2. Search for **"Google Drive API"**
3. Click on it → Click **"Enable"**

### Step 3: Configure OAuth Consent Screen
1. Go to **APIs & Services → OAuth consent screen**
2. Select **"External"** → Click **Create**
3. Fill in:
   - App name: `Prompt Master Notes`
   - User support email: your email
   - Developer contact email: your email
4. Click **Save and Continue** through all steps
5. Add your Google account as a **Test user** (on the Test users step)

### Step 4: Create OAuth 2.0 Client ID
1. Go to **APIs & Services → Credentials**
2. Click **"+ Create Credentials"** → **"OAuth client ID"**
3. Application type: **Web application**
4. Name: `Prompt Master Notes Web Client`
5. **Authorized JavaScript origins** — add both:
   - `http://localhost:5173` (for local development)
   - `https://your-app-name.vercel.app` (your production Vercel URL — add after deployment)
6. Click **Create**
7. **Copy the Client ID** — it looks like: `123456789.apps.googleusercontent.com`

### Step 5: Create an API Key
1. Still in **Credentials**, click **"+ Create Credentials"** → **"API Key"**
2. Copy the API key
3. (Optional but recommended) Click **"Restrict Key"**:
   - API restrictions → Restrict key → Select **"Google Drive API"**

### Step 6: Add to .env.local
```bash
VITE_GOOGLE_CLIENT_ID=123456789-xxxxxxxxxxxxxxxxxx.apps.googleusercontent.com
VITE_GOOGLE_API_KEY=AIzaSyXXXXXXXXXXXXXXXXXX
```

---

## 🌐 Deploy to Vercel

### Option A: Deploy via Vercel CLI (Recommended)

```bash
# Install Vercel CLI
npm install -g vercel

# Login to Vercel
vercel login

# Deploy (run from your project folder)
vercel

# Follow the prompts:
# - Link to existing project or create new
# - Framework: Vite
# - Build command: bun run build (or npm run build)
# - Output directory: dist
```

### Option B: Deploy via Vercel Dashboard

1. Push your code to GitHub
2. Go to [vercel.com](https://vercel.com) → **New Project**
3. Import your GitHub repository
4. Framework Preset: **Vite**
5. Build Command: `bun run build` (or `npm run build`)
6. Output Directory: `dist`
7. Click **Deploy**

### Add Environment Variables to Vercel
1. After deployment, go to your project in Vercel dashboard
2. **Settings → Environment Variables**
3. Add:
   - `VITE_GOOGLE_CLIENT_ID` = your client ID
   - `VITE_GOOGLE_API_KEY` = your API key
4. Click **Save** → **Redeploy** your project

### Update Authorized Origins
After getting your Vercel URL (e.g., `https://prompt-master-notes.vercel.app`):
1. Return to Google Cloud Console → **Credentials**
2. Edit your OAuth Client
3. Add the Vercel URL to **Authorized JavaScript origins**
4. Save

---

## 📁 Project Structure

```
src/
├── App.tsx                    # Main app with view routing
├── components/
│   ├── FolderView.tsx         # iOS-style folder list
│   ├── NoteListView.tsx       # Note list with pinned/today sections
│   ├── NoteEditor.tsx         # Tiptap rich text editor
│   └── SettingsView.tsx       # Google Drive backup/restore
├── store/
│   └── notesStore.ts          # Zustand store (persisted to localStorage)
├── types/
│   └── notes.ts               # TypeScript interfaces
└── lib/
    ├── googleDrive.ts          # Google Drive API integration
    └── nanoid.ts               # ID generator
```

---

## 🎨 Features

- **Pure Black iOS Theme** — #000000 background, #FFD60A gold accent
- **Folder Organization** — Create custom folders, All Notes, Pinned, Recently Deleted
- **Rich Text Editor** — Bold, Italic, Underline, Strikethrough, H1/H2/H3
- **Checklists** — Interactive iOS-style checklist items with circular toggles
- **Image Support** — Upload images directly into notes
- **Pinned Notes** — Pin important notes to the top
- **Real-time Search** — Filter notes instantly across all folders
- **Offline First** — All data saved to localStorage, works without internet
- **Google Drive Backup** — Backup & restore all notes via Google Drive appDataFolder
- **Framer Motion Animations** — iOS spring transitions between views

---

## 🔧 Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | React + Vite + TypeScript |
| Styling | Tailwind CSS + iOS design tokens |
| State | Zustand (with localStorage persistence) |
| Editor | Tiptap (ProseMirror-based) |
| Animations | Framer Motion |
| Cloud | Google Drive API v3 |
| Icons | Lucide React |

---

## ❓ Troubleshooting

**"Sign-in failed" on Google Drive backup?**
- Make sure your Client ID is correct in `.env.local`
- Add `http://localhost:5173` to Authorized JavaScript origins
- Add your Google account as a Test User in OAuth consent screen

**Notes not saving?**
- Check browser localStorage is not blocked
- Try clearing localStorage: `localStorage.clear()` in DevTools console, then refresh

**Build errors?**
- Run `bun install` to ensure all packages are installed
- Check Node.js version is 18+ and Bun is latest version
