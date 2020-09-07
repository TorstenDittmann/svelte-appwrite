# WIP: svelte-appwrite

Easy to use [Appwrite](https://appwrite.io/) components for Svelte. Install it:

```bash
npm install # or yarn
```

# Example

```svelte
<script>
  import { Init, User, OAuth2 } from "svelte-appwrite";
</script>

<main>
  <Init endpoint="http://localhost/v1" project="5f4938898667e" locale="de">
    <User let:user>
      <h1>Hello {user.name}!</h1>
      <div>{user.email}</div>

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
  </Init>
</main>
```
