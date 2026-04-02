# Deployment Guide

This project is currently a standard Flutter app with web support enabled. There is no checked-in Firebase Hosting configuration yet, so the first deployment includes a one-time Firebase setup.

## Recommended target

The most straightforward deployment path for this repo is:

- Build the Flutter web app
- Host the generated `build/web` output on Firebase Hosting

That gives you a fast static deployment with SPA routing support.

## Prerequisites

Before deploying, make sure you have:

- Flutter installed and available in your shell
- A Firebase project created in the Firebase console
- Node.js installed so you can use the Firebase CLI
- Permission to deploy to that Firebase project

Useful version checks:

```bash
flutter --version
dart --version
node --version
npm --version
```

## One-time setup

### 1. Install the Firebase CLI

```bash
npm install -g firebase-tools
```

### 2. Log in to Firebase

```bash
firebase login
```

### 3. Fetch Flutter packages

From the project root:

```bash
flutter pub get
```

### 4. Initialize Firebase Hosting in this repo

Run:

```bash
firebase init hosting
```

Use these answers when prompted:

- **Select a Firebase project**: choose your project
- **What do you want to use as your public directory?** `build/web`
- **Configure as a single-page app?** `Yes`
- **Set up automatic builds and deploys with GitHub?** `No` for now
- **File `build/web/index.html` already exists. Overwrite?** `No`

This should create:

- `firebase.json`
- `.firebaserc`

## Build for production

Create an optimized web build:

```bash
flutter build web --release
```

If you plan to host the app under a subpath instead of the domain root, build with a base href. Example:

```bash
flutter build web --release --base-href /refurrm-scan/
```

Use the default build command if the app will live at the root domain, such as `https://your-site.web.app/`.

## Deploy to Firebase Hosting

After the build completes:

```bash
firebase deploy --only hosting
```

When deployment succeeds, Firebase will print your live Hosting URLs.

## Preview before production

If you want a temporary preview URL first:

```bash
flutter build web --release
firebase hosting:channel:deploy preview
```

This is useful for QA or stakeholder review before pushing to production.

## Typical deployment workflow

For repeat deployments, the process is usually just:

```bash
flutter pub get
flutter build web --release
firebase deploy --only hosting
```

## Optional `firebase.json` check

After `firebase init hosting`, your `firebase.json` should look roughly like this:

```json
{
  "hosting": {
    "public": "build/web",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

The rewrite rule is important so Flutter web routes keep working when users refresh or open deep links directly.

## Common issues

### Blank page after deploy

Usually caused by one of these:

- The app was deployed to a subpath but built without `--base-href`
- `firebase.json` is missing the SPA rewrite to `/index.html`
- Old cached assets are still being served by the browser

Try rebuilding, redeploying, and then hard-refreshing the page.

### `firebase` command not found

Your global npm bin path may not be on `PATH`. Reinstall the CLI or open a new terminal after:

```bash
npm install -g firebase-tools
```

### Wrong Firebase project

Check the active project:

```bash
firebase use
```

Switch if needed:

```bash
firebase use --add
```

## Notes for this repo

- This repo does **not** currently include `firebase.json` or `.firebaserc`
- The app code appears to be the default Flutter starter app, so deployment is currently web-only unless you add mobile release configuration later
- Android and iOS store deployment are separate workflows and are not covered by this guide

## Suggested next step

Once you run `firebase init hosting`, it is a good idea to commit the generated `firebase.json` and `.firebaserc` so future deployments are consistent.
