<script>
  import Home from "./home.svelte";
  import { ENV } from "./pages/authentication/env";
  import { onMount } from 'svelte';
  let access_token;

  const login = () => {
    const authorizeUrl =  `${ENV.NIBE_MYUPLINK_DEV_URL}?response_type=code&client_id=${ENV.CLIENT_ID}&scope=${ENV.SCOPE}&redirect_uri=${ENV.REDIRECT_URI}&state=${ENV.STATE}`;
    fetch(authorizeUrl).then(response=>{
      window.location.replace(response.url);
    });
  }

  onMount(() => {
    access_token = sessionStorage.getItem("access_token");
    if (!access_token) {
      login();
    }
  });
</script>

{#if access_token}
<Home />
{/if}
