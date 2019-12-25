<script>
  import { onMount } from "svelte";
  const apiUrl = "https://api.github.com/users";
  let users = [];
  let loading = true;
  onMount(async () => {
    let userData = await fetch(apiUrl);
    //console.log(userData)
    let githubUsers = await userData.json();
    console.log(githubUsers);
    users = githubUsers;
    loading = false;
  });
</script>

<style>
  h2 {
    text-align: center;
  }
</style>

{#if loading}
  <h2>Loading...</h2>
{:else}
  <section>
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

  </section>
{/if}
