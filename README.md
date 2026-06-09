# MR Tracker — testops_mr-tracker

Offline-first Progressive Web App for pharmaceutical medical representatives to log field visits, sync to Google Sheets, capture GPS coordinates and photos.

## Architecture

| Component | Tech | Host |
|---|---|---|
| UI | React 18 + Vite + Tailwind + Dexie PWA | Hostinger `sapienoids.com/mr-tracker/` |
| Backend | Google Apps Script | Google's cloud (already deployed) |

## Local Development

```bash
cd ui
cp .env.example .env
# Edit .env — set VITE_APPS_SCRIPT_URL to your deployed Apps Script URL
npm install
npm run dev
```

## Build & Deploy (UI → Hostinger)

```bash
cd ui
cp .env.example .env   # fill VITE_APPS_SCRIPT_URL
npm ci
npm run build          # outputs to ui/dist/
```

Upload the contents of `ui/dist/` to `public_html/mr-tracker/` via Hostinger hPanel File Manager.  
The `.htaccess` in `dist/` handles React Router client-side routing on Apache.

## Backend (Google Apps Script)

`backend/Code.gs` is a reference copy. The live version runs in the Google Apps Script IDE at:
`https://script.google.com/macros/s/AKfycbxQ7HIztpGnAWRmhbTRvi-jFBOIChfZshH7fkTaBg81LDaDLfgmfXTOw5kqRJMWqMQ/exec`

To update the backend: edit in the Apps Script IDE, redeploy as Web App, update `VITE_APPS_SCRIPT_URL` in `.env` and rebuild.

## Features

- PIN-based auth (SHA-256 hashed, stored in Google Sheets)
- Offline-first via IndexedDB (Dexie) + background sync
- GPS geolocation with haversine distance calculation
- Voice input for visit notes
- Photo capture → Google Drive upload
- Data sync to Google Sheets (MR_Visits, MR_Employees, Products tabs)
- Daily summary and visit history views
- PWA installable on Android/iOS
