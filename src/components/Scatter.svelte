<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";
  let data = [
    { category: "setosa", x: 5.1, y: 3.5 },
    { category: "setosa", x: 4.9, y: 3 },
    { category: "setosa", x: 4.7, y: 3.2 },
    { category: "setosa", x: 4.6, y: 3.1 },
    { category: "setosa", x: 5, y: 3.6 },
    { category: "setosa", x: 5.4, y: 3.9 },
    { category: "setosa", x: 4.6, y: 3.4 },
    { category: "setosa", x: 5, y: 3.4 },
    { category: "setosa", x: 4.4, y: 2.9 },
    { category: "setosa", x: 4.9, y: 3.1 },
    { category: "setosa", x: 5.4, y: 3.7 },
    { category: "setosa", x: 4.8, y: 3.4 },
    { category: "setosa", x: 4.8, y: 3 },
    { category: "setosa", x: 4.3, y: 3 },
    { category: "setosa", x: 5.8, y: 4 },
    { category: "setosa", x: 5.7, y: 4.4 },
    { category: "setosa", x: 5.4, y: 3.9 },
    { category: "setosa", x: 5.1, y: 3.5 },
    { category: "setosa", x: 5.7, y: 3.8 },
    { category: "setosa", x: 5.1, y: 3.8 },
    { category: "setosa", x: 5.4, y: 3.4 },
    { category: "versicolor", x: 7, y: 3.2 },
    { category: "versicolor", x: 6.4, y: 3.2 },
    { category: "versicolor", x: 6.9, y: 3.1 },
    { category: "versicolor", x: 5.5, y: 2.3 },
    { category: "versicolor", x: 6.5, y: 2.8 },
    { category: "versicolor", x: 5.7, y: 2.8 },
    { category: "versicolor", x: 6.3, y: 3.3 },
    { category: "versicolor", x: 4.9, y: 2.4 },
    { category: "versicolor", x: 6.6, y: 2.9 },
    { category: "versicolor", x: 5.2, y: 2.7 },
    { category: "versicolor", x: 5, y: 2 },
    { category: "versicolor", x: 5.9, y: 3 },
    { category: "versicolor", x: 6, y: 2.2 },
    { category: "versicolor", x: 6.1, y: 2.9 },
    { category: "versicolor", x: 5.6, y: 2.9 },
    { category: "versicolor", x: 6.7, y: 3.1 },
    { category: "versicolor", x: 5.6, y: 3 },
    { category: "versicolor", x: 5.8, y: 2.7 },
    { category: "versicolor", x: 6.2, y: 2.2 },
    { category: "versicolor", x: 5.6, y: 2.5 },
    { category: "versicolor", x: 5.9, y: 3.2 },
    { category: "virginica", x: 6.3, y: 3.3 },
    { category: "virginica", x: 5.8, y: 2.7 },
    { category: "virginica", x: 7.1, y: 3 },
    { category: "virginica", x: 6.3, y: 2.9 },
    { category: "virginica", x: 6.5, y: 3 },
    { category: "virginica", x: 7.6, y: 3 },
    { category: "virginica", x: 4.9, y: 2.5 },
    { category: "virginica", x: 7.3, y: 2.9 },
    { category: "virginica", x: 6.7, y: 2.5 },
    { category: "virginica", x: 7.2, y: 3.6 },
    { category: "virginica", x: 6.5, y: 3.2 },
    { category: "virginica", x: 6.4, y: 2.7 },
    { category: "virginica", x: 6.8, y: 3 },
    { category: "virginica", x: 5.7, y: 2.5 },
    { category: "virginica", x: 5.8, y: 2.8 },
    { category: "virginica", x: 6.4, y: 3.2 },
    { category: "virginica", x: 6.5, y: 3 },
    { category: "virginica", x: 7.7, y: 3.8 },
    { category: "virginica", x: 7.7, y: 2.6 },
    { category: "virginica", x: 6, y: 2.2 },
    { category: "virginica", x: 6.9, y: 3.2 },
  ];

  onMount(async () => {
    let width = 600;
    let height = 600;
    const margin = { top: 25, right: 20, bottom: 35, left: 40 };

    const wrapper = d3.select("#scatter");

    const svg = wrapper.append("svg").attr("viewBox", [0, 0, width, height]);

    const x = d3
      .scaleLinear()
      .domain(d3.extent(data, (d) => d.x))
      .nice()
      .range([margin.left, width - margin.right]);

    const y = d3
      .scaleLinear()
      .domain(d3.extent(data, (d) => d.y))
      .nice()
      .range([height - margin.bottom, margin.top]);

    const xAxis = (g) =>
      g
        .attr("transform", `translate(0,${height - margin.bottom})`)
        .call(d3.axisBottom(x).ticks(width / 80))
        .call((g) => g.select(".domain").remove())
        .call((g) =>
          g
            .append("text")
            .attr("x", width)
            .attr("y", margin.bottom - 4)
            .attr("fill", "currentColor")
            .attr("text-anchor", "end")
            .text(data.x)
        );

    const yAxis = (g) =>
      g
        .attr("transform", `translate(${margin.left},0)`)
        .call(d3.axisLeft(y))
        .call((g) => g.select(".domain").remove())
        .call((g) =>
          g
            .append("text")
            .attr("x", -margin.left)
            .attr("y", 10)
            .attr("fill", "currentColor")
            .attr("text-anchor", "start")
            .text(data.y)
        );

    const grid = (g) =>
      g
        .attr("stroke", "currentColor")
        .attr("stroke-opacity", 0.1)
        .call((g) =>
          g
            .append("g")
            .selectAll("line")
            .data(x.ticks())
            .join("line")
            .attr("x1", (d) => 0.5 + x(d))
            .attr("x2", (d) => 0.5 + x(d))
            .attr("y1", margin.top)
            .attr("y2", height - margin.bottom)
        )
        .call((g) =>
          g
            .append("g")
            .selectAll("line")
            .data(y.ticks())
            .join("line")
            .attr("y1", (d) => 0.5 + y(d))
            .attr("y2", (d) => 0.5 + y(d))
            .attr("x1", margin.left)
            .attr("x2", width - margin.right)
        );
    // const color = d3.scaleOrdinal(
    //   data.map((d) => d.category),
    //   d3.schemeCategory10
    // );
    const color = d3.scaleOrdinal(
      data.map((d) => d.category),
      ["#999", "#d0c0da", "#0b2830"]
    );

    const shape = d3.scaleOrdinal(
      data.map((d) => d.category),
      d3.symbols.map((s) => d3.symbol().type(s)())
    );

    svg.append("g").call(xAxis);

    svg.append("g").call(yAxis);

    svg.append("g").call(grid);

    svg
      .append("g")
      .attr("stroke-width", 1.5)
      .attr("font-family", "sans-serif")
      .attr("font-size", 10)
      .selectAll("path")
      .data(data)
      .join("path")
      .attr("transform", (d) => `translate(${x(d.x)},${y(d.y)})`)
      .attr("fill", (d) => color(d.category))
      .attr("d", (d) => shape(d.category));
  });
</script>

<div class="graph">
  <div>
    <h3>Scatter Plot</h3>
    <p>Relationship between width and length of three species.</p>
  </div>
  <div id="scatter" />
</div>

<style>
  .graph {
    margin-top: 10rem;
  }
  h3 {
    text-align: center;
  }
  p {
    text-align: center;
    font-weight: 300;
    margin-bottom: 2rem;
  }
</style>
