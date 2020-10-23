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
  console.log(process.env.BASE_API_URL)
</script>


<h1>list of sessions</h1>

{#await sessionsList then { rooms }}
{#each rooms as room}
<li>
  <a href={$url('/rooms/:name', {name: room.name})}>{room.id} - {room.name}</a>
</li>
	{/each}
{/await}