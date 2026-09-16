### AuthCheck Component

#### AuthCheck Component

// +page.svelte
```svelte
<script lang="ts">
    import { user } from "$lib/firebase";
</script>
  
{#if $user}
  <slot />
{:else}
    <p class="text-error">
        You must be signed in to view this page.
        <a class="btn btn-primary" href="/login">Sign in</a>
    </p>
{/if}
```

#### Svelte 5 Version

```text
<script lang="ts">
  import { user } from "$lib/firebase";
  interface Props {
    children?: import('svelte').Snippet;
  }

  let { children }: Props = $props();
</script>
  
  {#if $user}
    {@render children?.()}
  {:else}
      <p class="text-error my-10">
          You must be signed in to view this page.
          <a class="btn btn-primary" href="/login">Sign in</a>
      </p>
  {/if}
```
