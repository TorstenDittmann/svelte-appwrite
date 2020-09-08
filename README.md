# WIP: svelte-appwrite

Easy to use [Appwrite](https://appwrite.io/) components for Svelte. Install it:

```bash
npm install # or yarn
```

# Example

## App.svelte
```svelte
<script>
  import { Appwrite, User, OAuth2 } from "svelte-appwrite";
  import TodoList from "./TodoList.svelte";

  const config = {
    endpoint: "http://localhost/v1",
    project: "5f4938898667e",
    locale: "de",
  };
</script>

<main>
  <Appwrite {...config}>
    <User let:user>
      <h1>Hello {user.name}!</h1>
      <div>{user.email}</div>
      <TodoList />
      <div slot="error">
        <OAuth2
          provider="discord"
          success="http://localhost:5000?success"
          failure="http://localhost:5000?failure"
          let:authorize>
          <button on:click={authorize}>Login Discord</button>
        </OAuth2>
      </div>
    </User>
  </Appwrite>
</main>
```
## TodoList.svelte

```svelte
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

```