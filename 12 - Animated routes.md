### Animated Routes

#### Animated Route Component

// lib/components/AnimatedRoute.svelte
```svelte
<script>
    import { fly} from "svelte/transition";
    import { page } from "$app/stores";
</script>

{#key $page.url}
  <div in:fly={{  x:'-100%', duration: 500 }}>
    <slot  />
  </div>
{/key}
```

#### Login Layout

// login/+layout.svelte
```svelte
<AnimatedRoute>
  <main>
    ...
  </main>
</AnimatedRoute>
```

#### Svelte 5 Version

// lib/components/AnimatedRoute.svelte
```svelte
<script>
    import { fly} from "svelte/transition";
    import { page } from "$app/stores";
  /** @type {{children?: import('svelte').Snippet}} */
  let { children } = $props();
</script>

{#key $page.url}
  <div in:fly={{  x:'-100%', duration: 500 }}>
    {@render children?.()}
  </div>
{/key}
```
