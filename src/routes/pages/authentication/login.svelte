<script>
  import { onMount } from 'svelte';
  import { ENV } from "./env"

  const getToken = (data) => {
    fetch(ENV.NIBE_MYUPLINK_DEV_TOKEN_URL, {
        method: "POST",
        headers: {
          "Content-Type": "application/x-www-form-urlencoded"
        },
        body: new URLSearchParams(data)
    }).then(response=>response.json()).then(response=>{
      for (const [key, value] of Object.entries(response)) {
        sessionStorage.setItem(key, value);
      }
      window.location.replace("/");
    })
  }

  onMount(()=>{
    const urlParams = new URLSearchParams(window.location.search);
    const data = {
      grant_type: "authorization_code",
      client_id: ENV.CLIENT_ID,
      client_secret: ENV.CLIENT_SECRET,
      code: urlParams.get('code'),
      redirect_uri: ENV.REDIRECT_URI
    }
    console.log(data);
    getToken(data);
  })
</script>
