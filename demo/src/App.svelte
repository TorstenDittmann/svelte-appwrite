<script>
  import { Appwrite, User, AuthOAuth2 } from "svelte-appwrite";
  import Login from "./Login.svelte";
  import TodoList from "./TodoList.svelte";

  const config = {
    endpoint: "http://localhost/v1",
    project: "5f4938898667e",
    locale: "de",
  };
</script>

<main>
  <Appwrite {...config}>
    <User let:user let:actions>
      <h1>Hello {user.name}!</h1>
      <div>{user.email}</div>
      <button on:click={actions.logout(actions.reload)}>Logout</button>
      <TodoList />
      <div slot="error">
        <AuthOAuth2
          provider="discord"
          success="http://localhost:5000?success"
          failure="http://localhost:5000?failure"
          let:authorize>
          <button on:click={authorize}>Login Discord</button>
        </AuthOAuth2>
        <AuthOAuth2
          provider="twitch"
          success="http://localhost:5000?success"
          failure="http://localhost:5000?failure"
          let:authorize>
          <button on:click={authorize}>Login Twitch</button>
        </AuthOAuth2>
        <AuthOAuth2
          provider="google"
          success="http://localhost:5000?success"
          failure="http://localhost:5000?failure"
          let:authorize>
          <button on:click={authorize}>Login Google</button>
        </AuthOAuth2>
        <hr />
        <Login on:success={actions.reload} />
      </div>
    </User>
  </Appwrite>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 640px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }
</style>
