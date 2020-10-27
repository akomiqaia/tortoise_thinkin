<script>
    import { url } from '@sveltech/routify'
    let sessionsList = fetchSessionData();
  
  async function fetchSessionData() {
    const res = await fetch(`${process.env.BASE_API_URL}/rooms`);
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
  </div>