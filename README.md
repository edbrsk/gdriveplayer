# GDrive Player

A lightweight, static web app that lets you browse and play videos directly from your Google Drive. Built as a single HTML file — no backend, no build step. Deployable to GitHub Pages.

## Features

- **Google Sign-In** — OAuth-based, no API key sharing needed
- **Google Drive Picker** — browse and select your root folder
- **Folder navigation** — nested folder support with back button
- **Video playback** — embedded Google Drive player with previous/next navigation
- **Progress tracking** — mark videos as Pending / In Progress / Watched (persisted in Firebase Firestore)
- **Search** — recursive folder search across all levels
- **In Progress tab** — quick access to courses you're currently watching
- **Session persistence** — stays logged in across page refreshes

## Setup

### 1. Google Cloud

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a project (or use existing)
3. Enable **Google Drive API** and **Google Picker API**
4. Go to **Credentials** → Create **OAuth 2.0 Client ID** (Web application)
5. Add your domain as an Authorized JavaScript origin (e.g. `https://yourusername.github.io`)
6. Add `http://localhost:8000` for local testing
7. In **OAuth consent screen** → add your email as a test user

### 2. Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a project (or add Firebase to your Google Cloud project)
3. Add a **Web app** → note the `apiKey` and `projectId`
4. Create **Cloud Firestore** database
5. Set security rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/progress/{videoId} {
      allow read, write: if true;
    }
  }
}
```

### 3. Deploy

```bash
gh repo create gdriveplayer --public --source=. --push
```

Then enable GitHub Pages in repo Settings → Pages → Source: `main` branch, `/ (root)`.

### 4. First run

1. Open the app and click **Configure Settings**
2. Enter your OAuth Client ID, Firebase API Key, and Project ID
3. Click Save — these are stored in your browser's localStorage

## Local development

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000`

## Security

- No secrets are hardcoded in the source code
- All configuration is stored in the user's browser (localStorage)
- Firebase config values (apiKey, projectId) are public client identifiers, not secrets
- Firestore data is scoped per user's Google ID
