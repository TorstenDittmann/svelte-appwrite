<script>
  import { onMount } from "svelte";
  import { active } from "./stores";
  import Appwrite from "./appwrite";

  export let endpoint;
  export let project;
  export let locale = "en";

  onMount(() => {
    if (endpoint == "") {
      console.error("Endpoint must be set.");
      return;
    }
    if (project == "") {
      console.error("Project ID must be set.");
      return;
    }

    Appwrite.setConfig({ endpoint, project, locale });
    $active = true;
  });
</script>

{#if $active}
  <slot />
{/if}
