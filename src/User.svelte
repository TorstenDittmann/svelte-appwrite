<script>
  import { createEventDispatcher } from "svelte";
  import { active, currentUser } from "./stores";
  import Appwrite from "./appwrite";

  const dispatch = createEventDispatcher();

  const fetchUser = async () => {
    try {
      const response = await Appwrite.sdk.account.get();
      $currentUser = response;
      dispatch("success", $currentUser);
      return response;
    } catch (error) {
      dispatch("failure", error);
      throw error;
    }
  };

  const actions = {
    reload: () => (request = fetchUser()),
    logout: async callback => {
      await Appwrite.sdk.account.deleteSession("current");
      callback();
    },
  };

  let request = fetchUser();
</script>

{#if $active}
  {#await request}
    <slot name="loading" />
  {:then user}
    <slot {user} {actions} />
  {:catch error}
    <slot name="error" {error} />
  {/await}
{/if}
