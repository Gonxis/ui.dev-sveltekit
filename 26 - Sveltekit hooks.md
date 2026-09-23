### SvelteKit Hooks

#### Auth Hook

// src/hooks.server.ts
```ts
import { adminAuth } from "$lib/server/admin";
import type { Handle } from "@sveltejs/kit";

export const handle = (async ({ event, resolve }) => {
  const sessionCookie = event.cookies.get("__session");

  try {
    const decodedClaims = await adminAuth.verifySessionCookie(sessionCookie!);
    event.locals.userID = decodedClaims.uid;
    console.log("found user id", decodedClaims.uid);
  } catch (e) {
    event.locals.userID = null;
    return resolve(event);
  }

  return resolve(event);
}) satisfies Handle;
```

#### Locals Type Definitions

// src/app.d.ts
```ts
// See https://kit.svelte.dev/docs/types#app
// for information about these interfaces
declare global {
	namespace App {
		// interface Error {}
		interface Locals {
			userID: string | null;
		}
		// interface PageData {}
		// interface Platform {}
	}
}

export {};
```

#### Refactored Data Fetching

// src/routes/[username]/bio/+page.server.ts
```ts
import type { PageServerLoad } from "./$types";
import { adminAuth, adminDB } from "$lib/server/admin";
import { error, redirect } from "@sveltejs/kit";

export const load = (async ({ locals, params }) => {

    const uid = locals.userID;

    if (!uid) {
      throw redirect(301, "/login");
    }
  
    const userDoc = await adminDB.collection("users").doc(uid).get();
    const { username, bio } = userDoc.data()!;
  
    if (params.username !== username) {
      throw error(401, "That username does not belong to you");
    }
  
  
    return {
      bio,
    };
}) satisfies PageServerLoad;
```
