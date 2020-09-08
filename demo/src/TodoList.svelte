<script>
  import { Collection, Document } from "svelte-appwrite";

  let value = "";
</script>

<Collection collection="5f56a3035a01f" let:documents let:actions>
  <form on:submit|preventDefault={actions.create({ label: value })}>
    <input type="text" bind:value />
  </form>
  <ul>
    {#each documents as document}
      <Document document={document} on:change={actions.reload} let:actions>
        <li>
          <input
            type="checkbox"
            checked={document.done}
            on:change={actions.update({ done: !document.done })} />
          {document.label}
          {#if document.done}
            <button on:click={actions.remove}>remove</button>
          {/if}
        </li>
      </Document>
    {/each}
  </ul>
</Collection>
