<script>
    import { metatags, url} from '@sveltech/routify'
    metatags.title = 'Tortoise Landing'
    metatags.description = 'Description coming soon...'
</script>

<h1>Tortoise Media ThinkIns</h1>

<h2>See all the availble ThinkIns</h2>

<div>

    <div>
      <p>
        Everyone who joins either the Host View or Participant View
        joins a single OpenTok session. Anyone with the Host View
        open can click Start Archive or Stop Archive to control
        recording of the entire session.
      </p>
    </div>
    <div>
        <button>
            <a href={$url(`/host`)}>Host View</a>

        </button>
        <button>
            <a href={$url(`/guest`)}>Participant View</a>
        </button>
    </div>
  </div>

