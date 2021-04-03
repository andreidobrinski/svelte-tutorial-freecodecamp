<script>
  import "../node_modules/materialize-css/dist/css/materialize.min.css";
  import "../node_modules/materialize-css/dist/js/materialize.min.js";
  import { Router, Link, Route } from "svelte-routing";
  import NavBar from "./layout/NavBar.svelte";
</script>

<main>
  <Router>
    <NavBar />
  </Router>
</main>

<style>
</style>
