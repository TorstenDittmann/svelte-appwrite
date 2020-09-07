<script>
  import { active } from "./stores";
  import Appwrite from "./appwrite";

  const user = Appwrite.sdk.account.get();
</script>

{#if $active}
  {#await user}
    <slot name="loading" />
  {:then current}
    <slot user={current} />
  {:catch error}
    <slot name="error" {error} />
  {/await}
{/if}
