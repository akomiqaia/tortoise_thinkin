<script>
    import { url } from '@sveltech/routify'
    let sessionsList = fetchData("/rooms");

    let archive_list = fetchData("/archives")
  
  async function fetchData(url) {
    const res = await fetch(`${process.env.BASE_API_URL}${url}`);
    const data = await res.json();

    if (res.ok) {
      return data;
    } else {
      throw new Error(data);
    }
  }



</script>


<div>

  
    <a href={$url(`/thinkins`)}>
      <button>
        New ThinkIn
      </button>
    </a>
    
    
    <h1>List of ThinkIns</h1>
    
    {#await sessionsList then { rooms }}
    {#each rooms as room}
    <li>
      <a href={$url('../live/:name', {name: room.name})}>{room.id} - {room.name}</a>
    </li>
    {/each}
    {/await}


    <h1>List of Recordings</h1>
    {#await archive_list then { archives }}
    {#each archives as archive}
    <li>
      <a href={$url(`https://tortoisearchive.s3.eu-west-2.amazonaws.com/46958944/${archive.id}/archive.mp4`)} target="_blank">Archive name - {archive.name}, Duration - {archive.duration} seconds</a>
    </li>
    {/each}
    {/await}
  </div>