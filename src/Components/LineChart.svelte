<script>
  import { onMount } from "svelte";
  import * as d3 from "d3";

  let exitVelocity = 67;
  let launchAngle = 0;
  let hoverText = "";
  let hoverX = 0;
  let hoverY = 0;

  let Hits = 0;
  let BBE = 0;
  let AvgDist = 0.0;
  let avg = 0.0;
  let wOBA = 0.0;
  let singles = 0;
  let doubles = 0;
  let triples = 0;
  let homeruns = 0;


  onMount(() => {
    drawDiamond();
  });

  function drawDiamond() {
    const width = 500;
    const height = 500;
    const margin = { top: 20, right: 20, bottom: 20, left: 20 };

    const svg = d3
      .select("#diamond")
      .attr("width", width)
      .attr("height", height)
      .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

    const fieldWidth = width - margin.left - margin.right;
    const fieldHeight = height - margin.top - margin.bottom;

    // Draw the green diamond
    svg
      .append("rect")
      .attr("x", fieldWidth / 8)
      .attr("y", fieldHeight / 8)
      .attr("width", (3 * fieldWidth) / 3)
      .attr("height", (3 * fieldHeight) / 3)
      .attr("fill", "green")
      .attr("transform", `rotate(225, ${fieldWidth / 2}, ${fieldHeight / 2})`);

    // Draw the tan diamond
    svg
      .append("rect")
      .attr("x", fieldWidth / 4)
      .attr("y", fieldHeight / 4)
      .attr("width", fieldWidth / 2)
      .attr("height", fieldHeight / 2)
      .attr("fill", "tan")
      .attr("transform", `rotate(45, ${fieldWidth / 2}, ${fieldHeight / 2})`);

    const baseSize = fieldWidth / 20;
    const halfBaseSize = baseSize / 2;

    // Add a group element for the hover text box
    const hoverBox = svg.append("g").style("visibility", "hidden");

    hoverBox
      .append("rect")
      .attr("id", "hoverBox")
      .attr("fill", "white")
      .attr("stroke", "black")
      .attr("rx", 5)
      .attr("ry", 5);

    hoverBox
      .append("text")
      .attr("id", "hoverText")
      .attr("text-anchor", "middle")
      .attr("dx", -10)
      .attr("dy", 12);

    // Draw bases (white diamonds with black outlines)
    const bases = [
      { x: fieldWidth / 2, y: fieldHeight / 4, label: "Double %: " },
      { x: 3 * fieldWidth / 4, y: fieldHeight / 2, label: "Single %: " },
      { x: fieldWidth / 2, y: 3 * fieldHeight / 4, label: "Home Run %: " },
      { x: fieldWidth / 4, y: fieldHeight / 2, label: "Triple %: " },
    ];

    bases.forEach((base) => {
      svg
        .append("rect")
        .attr("x", base.x - halfBaseSize)
        .attr("y", base.y - halfBaseSize)
        .attr("width", baseSize)
        .attr("height", baseSize)
        .attr("fill", "white")
        .attr("stroke", "black")
        .attr("transform", `rotate(45, ${base.x}, ${base.y})`)
        .on("mouseover", (event) => {
          const textWidth = base.label.length * 10; // Approximate text width
          const textHeight = 30; // Box height

          hoverBox
            .attr("transform", `translate(${base.x},${base.y - halfBaseSize - textHeight - 10})`)
            .style("visibility", "visible");

          hoverBox.select("rect")
            .attr("width", textWidth + 20)
            .attr("height", textHeight)
            .attr("x", -(textWidth / 2) - 10)
            .attr("y", -textHeight / 2);

          hoverBox.select("text")
            .attr("x", 0)
            .attr("y", -textHeight / 4)
            .text(base.label);
        })
        .on("mouseout", () => {
          hoverBox.style("visibility", "hidden");
        });
    });
  }
</script>

<svg id="diamond"></svg>

<div class="container">
  <div class="slider-container">
    <label for="exitVelocity">Exit Velocity</label>
    <input type="range" id="exitVelocity" min="12" max="122" bind:value={exitVelocity}>
    <span>{exitVelocity}</span>
    
    <label for="launchAngle">Launch Angle</label>
    <input type="range" id="launchAngle" min="-89" max="89" bind:value={launchAngle}>
    <span>{launchAngle}</span>
  </div>
  <div class="text-box">
    <h>Stats</h>
    <p>Hits: {Hits}</p> 
    <p>BBE: {BBE}</p>
    <p>Avg. Distance (ft): {AvgDist}</p>
    <p>Batting Avg.: {avg}</p>
    <p>wOBA: {wOBA}</p>
    <p>1B: {singles}</p>
    <p>2B: {doubles}</p>
    <p>3B: {triples}</p>
    <p>HR: {homeruns}</p>
  </div>
</div>



<style>

  .container {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    margin-top: 50px;
  }

    .text-box {
    margin-left: 20px;
    background-color: white;
    border: 1px solid black;
    padding: 10px;
    border-radius: 10px;
  }

  svg {
    display: block;
    margin: auto;
    margin-top: 50px;
  }
  
  .slider-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px;
  }
  
  .slider-container label {
    margin-top: 10px;
  }
  
  .slider-container input {
    margin: 5px 0;
  }
  
  .slider-container span {
    margin-bottom: 20px;
  }
  
  .hover-box {
    position: absolute;
    background-color: white;
    border: 1px solid black;
    padding: 20px;
    border-radius: 3px;
    pointer-events: none;
    margin-top: 20px;
  }
</style>
















