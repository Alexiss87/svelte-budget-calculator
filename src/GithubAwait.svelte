<script>
  import { onMount } from "svelte";
  const apiUrl = "https://api.github.com/users";

  async function getUsers() {
    let userData = await fetch(apiUrl);
    console.log(userData);
    let githubUsers = await userData.json();
    console.log(githubUsers);
    return githubUsers;
  }
</script>

<section>
  {#await getUsers()}
    loading...
  {:then users}
    {#each users as user, index (user.id)}
      <article class="user">
        <img src={user.avatar_url} alt={user.login} />
        <div class="user-info">
          <h3>User: {user.login}</h3>
          <a href="user.html-url" class="btn-primary" target="blank">
            github url
          </a>
        </div>
      </article>
    {/each}
  {:catch error}
    <h1>Somthing went wrong</h1>
    <h2>{error.message}</h2>
  {/await}
</section>
