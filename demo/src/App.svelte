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

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
