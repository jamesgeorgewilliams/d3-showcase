<script>
  import Header from "./components/Header.svelte";
  import Collision from "./components/Collision.svelte";
  import StickyForce from "./components/StickyForce.svelte";
  import Voronoi from "./components/Voronoi.svelte";
  import Scatter from "./components/Scatter.svelte";
</script>

<main>
  <div class="wrapper">
    <Header />
    <Collision />
    <StickyForce />
    <Voronoi />
    <Scatter />
  </div>
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    width: 100%;
  }
  .wrapper {
    max-width: 740px;
    margin-bottom: 10rem;
  }
</style>
