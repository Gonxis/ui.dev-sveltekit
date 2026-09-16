### OAuth SignIn

#### Basic SignIn Logic

```text
<script lang="ts">
    import { auth } from "$lib/firebase";

    import { GoogleAuthProvider, signInWithPopup, signOut } from "firebase/auth";


    async function signInWithGoogle() {
        const provider = new GoogleAuthProvider();
        const user = await signInWithPopup(auth, provider);
        console.log(user)
    }

</script>

<h2>Login</h2>

<button class="btn btn-primary" on:click={signInWithGoogle}>Sign in with Google</button>
```

#### Svelte 5 Version

```text
<button class="btn btn-primary" onclick={signInWithGoogle}>Sign in with Google</button>
```
