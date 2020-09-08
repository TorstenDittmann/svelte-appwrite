# WIP: svelte-appwrite

Easy to use [Appwrite](https://appwrite.io/) components for Svelte. Install it:

```bash
npm install # or yarn
```

# Usage

## Initialize

Must be initialised and wrap every `svelte-appwrite` component.

```svelte
<script>
  import { Appwrite } from "svelte-appwrite";

  const config = {
    endpoint: "http://localhost/v1",
    project: "5f4938898667e",
    locale: "de",
  };
</script>

<Appwrite {...config}>
  ...
</Appwrite>
```

## Login Email

Login via email and password.

```svelte
<script>
  import { AuthEmail } from "svelte-appwrite";

  let email = "";
  let password = "";

  const success = e => {
    //success callback
    // `e` contains the user object
  };

  const failure = e => {
    //failure callback
  }
</script>

<AuthEmail let:authorize on:success on:failure>
  <input type="text" bind:value={email}>
  <input type="text" bind:value={password}>
  <button on:click={authorize(email,password)}>Login</button>
</AuthEmail>
```

## Login OAuth2

Login via an OAuth2 provider.

```svelte
<script>
  import { AuthOAuth2 } from "svelte-appwrite";
</script>

<AuthOAuth2
  provider="google"
  success="http://localhost:5000?success"
  failure="http://localhost:5000?failure"
  let:authorize>
  <button on:click={authorize}>Login Google</button>
</AuthOAuth2>
```

## Get user

Requests current user to check if logged in.

```svelte
<script>
  import { User } from "svelte-appwrite";
</script>

<User let:user>
  <h1>Hello {user.name}!</h1>
  <div>{user.email}</div>

  <div slot="error">
    You are not logged in!
  </div>
</User>
```

## Get Collection

Get a list of all the user documents from a collection.

```svelte
<script>
  import { Collection } from "svelte-appwrite";
</script>

<Collection collection="5f56a3035a01f" let:documents>
  You have {documents.length} documents :)
</Collection>
```

## Get Document

Get a document. If you pass the `document` property with data from <Collection />, there wont be any data fetched.

```svelte
<script>
  import { Document } from "svelte-appwrite";
</script>

<Document id="5f56a3asda01f" let:document>
  Title: {document.title}
  Text: {document.text}
</Document>
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

<Collection id="5f56a3035a01f" let:documents let:actions>
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