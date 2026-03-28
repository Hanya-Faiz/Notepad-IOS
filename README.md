# Prompt Master Notes 📝

A high-fidelity iOS Notes clone built with React + Vite + TypeScript. Pure black theme, gold accent, Tiptap rich text editor, Google Drive backup.

## Features

- **iOS-accurate UI** — Pure black (#000000) background, iOS gold (#FFD60A) accent, Inter font
- **Folder system** — Create, rename custom folders with note counts
- **Rich text editor** — Powered by Tiptap with Bold, Italic, Underline, Strikethrough, Headings
- **Checklists** — Interactive iOS-style checklist bubbles
- **Image support** — Upload and paste images into notes
- **Pinned notes** — Pin important notes to top of list
- **Real-time search** — Filter notes instantly across all folders
- **Offline first** — All notes saved to localStorage automatically
- **Google Drive backup** — Encrypted private backup via `appDataFolder` scope
- **Framer Motion animations** — iOS Spring transitions between views

## Tech Stack

- React 18 + Vite + TypeScript
- Tailwind CSS + custom iOS design tokens
- Zustand (state + localStorage persistence)
- Tiptap (rich text editor)
- Framer Motion (animations)
- date-fns (date formatting)
- react-hot-toast (notifications)
- Google Drive API v3

---

## Setup

### 1. Clone & Install

```bash
git clone https://github.com/YOUR_USERNAME/prompt-master-notes.git
cd prompt-master-notes
npm install   # or: bun install
```

### 2. Configure Environment Variables

Copy `.env.example` to `.env.local`:

```bash
cp .env.example .env.local
```

Edit `.env.local` with your Google credentials:

```env
VITE_GOOGLE_CLIENT_ID=xxxxxxxx.apps.googleusercontent.com
VITE_GOOGLE_API_KEY=AIzaxxxxxxxxxxxxxxxxxxxxxxxx
```

### 3. Run Locally

```bash
npm run dev
# App opens at http://localhost:5173
```

---

## Google Cloud Console Setup (Step by Step)

### Step 1 — Create a Google Cloud Project

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Click **"Select a project"** → **"New Project"**
3. Name it `Prompt Master Notes` → **Create**

### Step 2 — Enable Google Drive API

1. Go to **APIs & Services** → **Library**
2. Search for **"Google Drive API"** → Click it → **Enable**

### Step 3 — Create OAuth 2.0 Client ID

1. Go to **APIs & Services** → **Credentials**
2. Click **"+ Create Credentials"** → **OAuth client ID**
3. If prompted, configure the **OAuth consent screen** first:
   - User Type: External
   - App name: `Prompt Master Notes`
   - Add scope: `https://www.googleapis.com/auth/drive.appdata`
4. Back in Credentials → Create OAuth client ID:
   - Application type: **Web application**
   - Name: `Prompt Master Web`
   - Authorized JavaScript origins:
     - `http://localhost:5173` (local dev)
     - `https://your-domain.vercel.app` (production)
5. Copy the **Client ID** → paste as `VITE_GOOGLE_CLIENT_ID`

### Step 4 — Create API Key

1. Click **"+ Create Credentials"** → **API Key**
2. (Optional) Restrict to Google Drive API
3. Copy the key → paste as `VITE_GOOGLE_API_KEY`

---

## Deploy to Vercel

### Option A — Vercel CLI

```bash
npm install -g vercel
vercel login
vercel --prod
```

During setup, add environment variables when prompted:
- `VITE_GOOGLE_CLIENT_ID` = your client ID
- `VITE_GOOGLE_API_KEY` = your API key

### Option B — Vercel Dashboard

1. Go to [vercel.com](https://vercel.com) → **New Project**
2. Import your GitHub repository
3. In **Environment Variables**, add:
   - `VITE_GOOGLE_CLIENT_ID`
   - `VITE_GOOGLE_API_KEY`
4. Click **Deploy**

After deployment, go back to Google Cloud Console and add your Vercel domain to **Authorized JavaScript Origins**.

---

## Deploy to GitHub

```bash
git init
git add .
git commit -m "Initial: Prompt Master Notes iOS clone"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/prompt-master-notes.git
git push -u origin main
```

---

## Project Structure

```
src/
├── App.tsx                 # Main app — AnimatePresence view router
├── index.css               # iOS pure black design tokens + Tiptap styles
├── components/
│   ├── FolderView.tsx      # Folder list (iOS style with counts + chevrons)
│   ├── NoteListView.tsx    # Note list with pinned/today/older sections
│   ├── NoteEditor.tsx      # Tiptap rich text editor + formatting toolbar
│   └── SettingsView.tsx    # Google Drive backup + setup guide
├── store/
│   └── notesStore.ts       # Zustand store with localStorage persistence
├── lib/
│   ├── googleDrive.ts      # Google Drive API v3 backup/restore
│   └── nanoid.ts           # Lightweight ID generator
└── types/
    └── notes.ts            # Note, Folder, View TypeScript types
```

---

## Notes

- Google Drive uses `appDataFolder` scope — backups are **private to this app only**
- Without `VITE_GOOGLE_CLIENT_ID`, the app still works fully offline
- Notes persist in `localStorage` under key `prompt-master-notes`
- Max note image size depends on browser localStorage limits (~5MB typical)
