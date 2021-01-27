<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  const graph = {
    nodes: Array.from({ length: 13 }, () => ({})),
    links: [
      { source: 0, target: 1 },
      { source: 1, target: 2 },
      { source: 2, target: 0 },
      { source: 1, target: 3 },
      { source: 3, target: 2 },
      { source: 3, target: 4 },
      { source: 4, target: 5 },
      { source: 5, target: 6 },
      { source: 5, target: 7 },
      { source: 6, target: 7 },
      { source: 6, target: 8 },
      { source: 7, target: 8 },
      { source: 9, target: 4 },
      { source: 9, target: 11 },
      { source: 9, target: 10 },
      { source: 10, target: 11 },
      { source: 11, target: 12 },
      { source: 12, target: 10 },
    ],
  };

  onMount(async () => {
    const width = 350;
    const height = 350;

    const wrapper = d3.select("#stickyForce");

    const svg = wrapper.append("svg").attr("viewBox", [0, 0, width, height]);

    const link = svg
      .append("g")
      .attr("stroke", "#999")
      .attr("stroke-opacity", 0.6)
      .selectAll(".link")
      .data(graph.links)
      .join("line")
      .classed("link", true);

    const node = svg
      .append("g")
      .attr("stroke", "black")
      .attr("stroke-width", 1)
      .selectAll(".node")
      .data(graph.nodes)
      .join("circle")
      .attr("r", 12)
      .attr("fill", "cornflowerblue")
      .classed("node", true)
      .classed("fixed", (d) => d.fx !== undefined);

    const simulation = d3
      .forceSimulation()
      .nodes(graph.nodes)
      .force("charge", d3.forceManyBody())
      .force("center", d3.forceCenter(width / 2, height / 2))
      .force("link", d3.forceLink(graph.links))
      .on("tick", tick);

    const drag = d3.drag().on("start", dragstart).on("drag", dragged);

    node.call(drag).on("click", click);

    function tick() {
      link
        .attr("x1", (d) => d.source.x)
        .attr("y1", (d) => d.source.y)
        .attr("x2", (d) => d.target.x)
        .attr("y2", (d) => d.target.y);
      node.attr("cx", (d) => d.x).attr("cy", (d) => d.y);
    }

    function click(event, d) {
      delete d.fx;
      delete d.fy;
      d3.select(this).classed("fixed", false);
      simulation.alpha(1).restart();
    }

    function dragstart() {
      d3.select(this).classed("fixed", true);
    }

    function dragged(event, d) {
      d.fx = clamp(event.x, 0, width);
      d.fy = clamp(event.y, 0, height);
      simulation.alpha(1).restart();
    }

    function clamp(x, lo, hi) {
      return x < lo ? lo : x > hi ? hi : x;
    }
  });
</script>

<div class="force">
  <div>
    <h2>Network data graph</h2>
    <p>
      Click and drag nodes out of position. Click the node again to make it
      return to the original position.
    </p>
  </div>
  <div id="stickyForce" />
</div>

<style>
  .force {
    margin-top: 10rem;
  }
  h2 {
    color: #333;
    text-align: center;
    font-weight: 100;
  }
  p {
    color: #333;
    text-align: center;
    font-weight: 300;
  }
</style>
