### Multi-step Forms

#### Create Routes

* login/+page.svelte
* login/+layout.svelte
* login/photo/+page.svelte
* login/username/+page.svelte

#### Root Layout

// +layout.svelte
```svelte
<div class="min-h-screen flex flex-col">
    <slot />
</div>
```

#### Login Layout

// login/+layout.svelte
```svelte
<script lang="ts">
  import { page } from "$app/stores";
</script>

<nav class="flex justify-center my-6">
  <ul class="steps">
    <a href="/login" class="step step-primary">Sign In</a>
    <a
      href="/login/username"
      class="step"
      class:step-primary={$page.route.id?.match(/username|photo/g)}>
      Choose Username
    </a>
    <a
      href="/login/photo"
      class="step"
      class:step-primary={$page.route.id?.includes("photo")}>
      Upload Photo
    </a>
  </ul>
</nav>

<main class="card w-4/6 bg-neutral text-neutral-content mx-auto">
  <div class="card-body items-center text-center">
    <slot />
  </div>
</main>
```

#### Svelte 5 Version

```text
<script lang="ts">
   interface Props {
      children?: import('svelte').Snippet;
   }

   let { children }: Props = $props();
</script>

<div class="min-h-screen flex flex-col">
    {@render children?.()}
</div>
```
