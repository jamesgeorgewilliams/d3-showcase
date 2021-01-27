<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  function createGraph() {
    const height = 500;
    const width = height;
    const radii = Array.from({ length: 150 }, d3.randomUniform(4, 18));

    let dpi = devicePixelRatio;
    const canvas = document.getElementById("myCanvas");
    canvas.width = width * dpi;
    canvas.height = height * dpi;
    canvas.style.width = width + "px";

    var context = canvas.getContext("2d");
    context.scale(dpi, dpi);
    const nodes = radii.map((r) => ({ r }));

    function ticked() {
      context.clearRect(0, 0, width, height);
      context.save();
      context.translate(width / 2, height / 2);
      context.beginPath();
      for (const d of nodes) {
        context.moveTo(d.x + d.r, d.y);
        context.arc(d.x, d.y, d.r, 0, 2 * Math.PI);
      }
      context.fillStyle = "lightblue";
      context.fill();
      context.strokeStyle = "#333";
      context.stroke();
      context.restore();
    }

    const simulation = d3
      .forceSimulation(nodes)
      .velocityDecay(0.2)
      .force("x", d3.forceX().strength(0.002))
      .force("y", d3.forceY().strength(0.002))
      .force(
        "collide",
        d3
          .forceCollide()
          .radius((d) => d.r + 0.5)
          .iterations(2)
      )
      .on("tick", ticked);
  }

  onMount(async () => {
    createGraph();
  });
</script>

<div class="collision">
  <div class="collision__left">
    <p>Click "Replay" to see animation</p>
    <button on:click={createGraph}>Replay</button>
  </div>
  <canvas id="myCanvas" />
</div>

<style>
  .collision {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .collision__left p {
    color: #333;
    font-weight: 300;
  }

  @media (max-width: 640px) {
    .collision {
      flex-direction: column;
    }
  }
</style>
