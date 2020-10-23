<script>
    import { metatags, url } from '@sveltech/routify'
    metatags.title = 'Tortoise Landing'
    metatags.description = 'Description coming soon...'
</script>

<h1>This is the landing page</h1>

<h2>See all the availble ThinkIns</h2>

<a href={$url(`/rooms`)}>Show me ThinkIns</a>