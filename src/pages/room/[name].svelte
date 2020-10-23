<script>
    import Video from "../../components/video/Video.svelte";
    import { params } from '@sveltech/routify'
    console.log($params.name)
    let sessionData = fetchSessionData();
  
    async function fetchSessionData() {
      const res = await fetch(`https://tortoise-thinkin.herokuapp.com/room/${$params.name}`);
      const data = await res.json();
  
      if (res.ok) {
        return data;
      } else {
        throw new Error(data);
      }
    }
  
    function handleClick() {
      sessionData = fetchSessionData();
    }
  </script>
  
  <h1>session info</h1>
  
  {#await sessionData then { sessionId, token, apiKey }}
    <!-- <p>{token}</p> -->
    <p>{sessionId}</p>
    <p>{apiKey}</p>
  {/await}
  
  {#await sessionData then { sessionId, token, apiKey }}
    <Video {apiKey} {sessionId} {token} />
  {/await}
  
  <button on:click={handleClick}> get session info </button>
  