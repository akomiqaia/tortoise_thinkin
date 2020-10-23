<script>
    import Video from "../../components/video/Video.svelte";
    import { params } from '@sveltech/routify'

    let sessionData = fetchSessionData();
  
    async function fetchSessionData() {
      const res = await fetch(`${process.env.BASE_API_URL}/room/${$params.name}`);
      const data = await res.json();
  
      if (res.ok) {
          console.log(data)
        return data;
      } else {
        throw new Error(data);
      }
    }
  </script>
  
  <h1>session {$params.name}</h1>
  
  {#await sessionData then { sessionId, token, apiKey }}
    <Video {apiKey} {sessionId} {token} />
  {/await}
  
  {#await sessionData then { sessionId, token, apiKey }}
    <!-- <p>{token}</p> -->
    <p>{sessionId}</p>
    <p>{apiKey}</p>
  {/await}
  
  