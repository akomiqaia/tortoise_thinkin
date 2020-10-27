<script>
  import { params } from "@sveltech/routify";

  import Stream from "../../../components/video/Stream.svelte";

  let state = {
    recording: false,
    recordingOptions: {},
    archiveId: null,
  };

  let sessionData = fetchSessionData();

  async function fetchSessionData() {
    const res = await fetch(`${process.env.BASE_API_URL}/room/${$params.name}`);
    const data = await res.json();

    if (res.ok) {
      console.log(data);
      state.recordingOptions.sessionId = data.sessionId;
      return data;
    } else {
      throw new Error(data);
    }
  }

  function handleRecording() {
    state.recording = !state.recording;

    if (state.recording) {
      state.recordingOptions = {
        ...state.recordingOptions,
        hasAudio: true,
        hasVideo: true,
        archiveName: $params.name,
        outputMode: "composed",
      };
      console.log("Recording started");

      fetch(`${process.env.BASE_API_URL}/startrecording`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(state.recordingOptions),
      })
        .then((data) => data.json())
        .then((data) => {
          console.log("Archive Data from server", data.archive.id);
          return state.archiveId = data.archive.id
        });

      console.log("state after post", state);
    } else {
      if (state.archiveId) {
        fetch(`${process.env.BASE_API_URL}/stoprecording/${state.archiveId}`);
        console.log("recording stopped");
        console.log(state);
      } else {
        console.log("there was an error");
      }
    }
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
    <button
      on:click={handleRecording}>{state.recording ? 'Stop Recording' : 'Start Recording'}</button>
  </div>
</div>
