<!-- InstagramLogin.svelte -->

<script>
  const appId = '520692423539156';
  const appSecret = '9189ef418496c1b32f5a587b7161239d';
  const redirectUri = 'https://jonathon-eng.vercel.app/menu/redirect';

  function loginWithInstagram() {
    const authUrl = `https://api.instagram.com/oauth/authorize?app_id=${appId}&redirect_uri=${redirectUri}&scope=user_profile,user_media&response_type=code`;
    window.open(authUrl, '_blank', 'width=600,height=600');
  }

  import { onMount } from 'svelte';

  onMount(() => {
    // this code will only be executed in the browser
    const params = new URLSearchParams(window.location.search);
    const code = params.get('code');
    if (code) {
      fetchInstagramToken(code);
    }
  });

  function handleInstagramRedirect() {
    // this code will only be executed in the browser
    const params = new URLSearchParams(window.location.search);
    const code = params.get('code');
    if (code) {
      fetchInstagramToken(code);
    }
  }

  function fetchInstagramToken(code) {
    // this code will only be executed in the browser
    console.log('fetchInstagramToken called with code', code);
    const url = `https://api.instagram.com/oauth/access_token`;
    const formData = new FormData();
    formData.append('app_id', appId);
    formData.append('app_secret', appSecret);
    formData.append('grant_type', 'authorization_code');
    formData.append('redirect_uri', redirectUri);
    formData.append('code', code);

    fetch(url, {
      method: 'POST',
      body: formData,
    })
      .then((response) => response.json())
      .then((data) => console.log(data))
      .catch((error) => console.error(error));
  }
</script>

{#if typeof window !== 'undefined'}
  <button on:click={loginWithInstagram}>Login with Instagram</button>
{/if}
