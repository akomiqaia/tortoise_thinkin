<script>
  import { Router } from "@sveltech/routify";
  import { routes } from "../.routify/routes";
</script>

<svelte:head>
  <script src="https://static.opentok.com/v2/js/opentok.min.js"></script> 
</svelte:head>

<style  global>
  @import "../static/global.css";
</style>

<Router {routes} />