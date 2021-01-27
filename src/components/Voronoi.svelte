<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  onMount(async () => {
    let data;
    let width = 600;
    let height = 600;
    const randomX = d3.randomNormal(width / 2, 80);
    const randomY = d3.randomNormal(height / 2, 80);
    data = d3
      .range(200)
      .map(() => [randomX(), randomY()])
      .filter((d) => 0 <= d[0] && d[0] <= width && 0 <= d[1] && d[1] <= height);

    const wrapper = d3.select("#voronoi");

    const svg = wrapper.append("svg").attr("viewBox", [0, 0, width, height]);

    const delaunay = d3.Delaunay.from(data);
    const voronoi = delaunay.voronoi([-1, -1, width + 1, height + 1]);

    const orient = {
      top: (text) => text.attr("text-anchor", "middle").attr("y", -6),
      right: (text) =>
        text.attr("text-anchor", "start").attr("dy", "0.35em").attr("x", 6),
      bottom: (text) =>
        text.attr("text-anchor", "middle").attr("dy", "0.71em").attr("y", 6),
      left: (text) =>
        text.attr("text-anchor", "end").attr("dy", "0.35em").attr("x", -6),
    };

    const cells = data.map((d, i) => [d, voronoi.cellPolygon(i)]);

    svg
      .append("g")
      .attr("stroke", "orange")
      .selectAll("path")
      .data(cells)
      .join("path")
      .attr("d", ([d, cell]) => `M${d3.polygonCentroid(cell)}L${d}`);

    svg
      .append("path")
      .attr("fill", "none")
      .attr("stroke", "#ccc")
      .attr("d", voronoi.render());

    svg.append("path").attr("d", delaunay.renderPoints(null, 2));

    svg
      .append("g")
      .style("font", "10px sans-serif")
      .selectAll("text")
      .data(cells)
      .join("text")
      .each(function ([[x, y], cell]) {
        const [cx, cy] = d3.polygonCentroid(cell);
        const angle =
          (Math.round((Math.atan2(cy - y, cx - x) / Math.PI) * 2) + 4) % 4;
        d3.select(this).call(
          angle === 0
            ? orient.right
            : angle === 3
            ? orient.top
            : angle === 1
            ? orient.bottom
            : orient.left
        );
      })
      .attr("transform", ([d]) => `translate(${d})`)
      .attr("display", ([, cell]) =>
        -d3.polygonArea(cell) > 2000 ? null : "none"
      )
      .text((d, i) => i);
  });
</script>

<div class="cell">
  <div>
    <h2>Voronoi Diagram</h2>
    <p>The area of each cell determines whether the label is visible.</p>
  </div>
  <div id="voronoi" />
</div>

<style>
  .cell {
    margin-top: 5rem;
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
    margin-bottom: 5rem;
  }
</style>
