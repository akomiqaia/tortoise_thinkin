<script>
  import Stream from "../../../components/video/Stream.svelte";
  import { params } from "@sveltech/routify";

  let sessionData = fetchSessionData();

  async function fetchSessionData() {
    const res = await fetch(`${process.env.BASE_API_URL}/room/${$params.name}`);
    const data = await res.json();

    if (res.ok) {
      console.log(data);
      return data;
    } else {
      throw new Error(data);
    }
  }

  let recording = false
  function handleRecording() {
    recording = !recording
    console.log(`recording state is ${recording}`)
  }
</script>

<style>
  .parent {
    display: grid;
    grid-template-columns: 240px 1fr 240px;
    grid-template-rows: 50px repeat(3, 1fr) 50px;
    grid-column-gap: 0px;
    grid-row-gap: 0px;
    height: 90vh;
  }

  .header {
    grid-area: 1 / 1 / 2 / 4;
    background-color: aqua;
  }
  .participants {
    grid-area: 2 / 1 / 5 / 2;
    background-color: yellow;
  }
  .video-panel {
    grid-area: 2 / 2 / 4 / 3;
    background-color: red;
  }
  .chat {
    grid-area: 2 / 3 / 5 / 4;
    background-color: green;
  }
  .footer {
    grid-area: 5 / 1 / 6 / 4;
    background-color: blue;
  }
  .clips {
    grid-area: 4 / 2 / 5 / 3;
    background-color: aqua;
  }
</style>



<div class="parent">
  <div class="header">ThinkIns - {$params.name}</div>
  <div class="participants">Participants</div>
  <div class="video-panel">
    {#await sessionData then { sessionId, token, apiKey }}
      <Stream {apiKey} {sessionId} {token} />
    {/await}
  </div>
  <div class="chat">Chat</div>
  <div class="clips">Clips</div>
  <div class="footer">
    <button on:click={handleRecording}>{recording ? 'Stop Recording' : 'Start Recording'}</button>
  </div>
</div>

{#await sessionData then { sessionId, token, apiKey }}
  <!-- <p>{token}</p> -->
  <p>{sessionId}</p>
  <p>{apiKey}</p>
{/await}
