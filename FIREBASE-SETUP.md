# JustBeng Class Book — Firebase Setup

This version includes Firebase Email/Password Authentication and cloud synchronization.

## 1. Enable Authentication

Firebase Console → **Authentication** → **Sign-in method** → **Email/Password** → Enable.

Leave **Email link (passwordless sign-in)** disabled unless you specifically want it.

## 2. Add the Firebase Web App configuration

In Firebase Console:

**Project settings → General → Your apps → Web app**

If you have not registered a Web App yet, click **Add app → Web**.

Copy the `firebaseConfig` object values into the `firebaseConfig` object near the top of `index.html`.

The project-specific values already filled in are:

- `projectId`: `justbeng-class-book-6dcb0`
- `databaseURL`: `https://justbeng-class-book-6dcb0-default-rtdb.firebaseio.com`

You still need to replace these placeholders:

- `apiKey`
- `storageBucket` (not used by the app, but included in the config)
- `messagingSenderId`
- `appId`

Firebase web API keys are client-side configuration values; do not put a server/admin private key into this file.

## 3. Publish secure Realtime Database rules

In Firebase Console → **Realtime Database → Rules**, use:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid"
      }
    }
  }
}
```

Then click **Publish**.

These rules match the app's storage path:

`users/{authenticated-user-uid}/classBook`

Each signed-in account can only read and write its own Class-Book data.

## 4. Deploy

The project remains GitHub Pages/PWA-ready.

After changing `index.html`, commit/push the project to GitHub and open the deployed site.

## 5. First login

Open the deployed app:

1. Choose **Create Account**.
2. Enter an email and password.
3. The app creates the Firebase account.
4. Your Class-Book data is stored under your Firebase UID.
5. Sign in with the same account on another device to load the same classes/profile/settings.

## Local data migration

If the previous version of the app already has classes saved in browser local storage, the first Firebase account you create on that browser will automatically receive that existing local data once.

After that, other Firebase accounts will not inherit the first account's local data.
