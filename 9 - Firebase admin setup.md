### Firebase Admin Setup

⚠️ Important! The Admin SDK can only be used on the server, so files named `+server.ts` or `+page.server.ts`.

1. Download service account from Firebase project settings
2. Add to .gitignore to be safe
3. Create env variable
4. Initialize SDK

```bash
npm i firebase-admin
```

#### Env Variable

Use an env variable because the location of the service account will change in production.

// .env
```env
FIREBASE_SERVICE_ACCOUNT="service-account.json"
FB_PROJECT_ID="..."
FB_CLIENT_EMAIL="....gserviceaccount.com"
FB_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\ ... \n-----END PRIVATE KEY-----\n"
```

#### Admin SDK Setup

This setup is optional if you want to use SSR auth. Make sure this file lives in the `lib/server` folder to prevent it from being included in client-side code.

// lib/server/admin.ts
```ts
import { getAuth } from 'firebase-admin/auth';
import { getFirestore } from 'firebase-admin/firestore';
import { FB_CLIENT_EMAIL, FB_PRIVATE_KEY, FB_PROJECT_ID, FB_SERVICE_ACCOUNT } from '$env/static/private'
import pkg from 'firebase-admin';

try {
  pkg.initializeApp({
    credential: pkg.credential.cert({
      projectId: FB_PROJECT_ID,
      clientEmail: FB_CLIENT_EMAIL,
      privateKey: FB_PRIVATE_KEY,
    }),
  });
} catch (err) {
  if (!/already exists/u.test(err.message)) {
    console.error('Firebase Admin Error: ', err.stack)
  }
}


export const adminDB = getFirestore();
export const adminAuth = getAuth();
```
