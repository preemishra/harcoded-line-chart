<script>
    import * as d3 from "d3";
    import { scaleLinear } from "d3-scale";
    import { max } from "d3-array";
    export let data
    import Tooltip from "./components/Tooltip.svelte";
    import Yaxis from "./components/Yaxis.svelte";
    import Xaxis from "./components/Xaxis.svelte";

    let width = 1500;
    let height = 800;
    let margin = { top: 80, right: 40, left: 0, bottom: 80 };
  
    $: xScale = scaleLinear()
      .domain([0, 100])
      .range([0, width - margin.left - margin.right]);
  
    let yScale = scaleLinear()
      .domain([0, max(data, (d) => d.hours)])
      .range([height - margin.top - margin.bottom, 0]);
  
    
    let path = d3
      .line()
      .x((d) => xScale(d.grade))
      .y((d) => yScale(d.hours))
      .curve(d3.curveNatural);

      let hoveredData;
    $: console.log(hoveredData);
  </script>
  
  <div
    class="container"
    on:mouseleave={() => {
      hoveredData = null;
    }}
  >
  <h1>Student study hours VS Scored grade</h1>
    <svg {width} {height}>
      <Xaxis {height} {xScale} {margin} />
      <Yaxis {height} {width} {yScale} {margin} />

      <g class="circles" transform="translate({margin.left} {margin.top})">
        <text x="1" y="-60" fill="brown" font-size="2em" transform="rotate(90 50,-20)">Study Hours</text>
        <text x="650" y="700" fill="brown" font-size="2em">Scored Grade</text>
        <path d={path(data.sort((a,b) => a.grade - b.grade))} fill="none" stroke="green" stroke-width=5 />
        {#each data.sort((a,b) => a.grade - b.grade) as student}
        <circle cx={xScale(student.grade)} 
                cy={yScale(student.hours)} 
                r={hoveredData && hoveredData == student ? "20" : "10"}
                opacity={hoveredData ? hoveredData == student ? "1" : ".3" : "1"}
                fill="brown"
                stroke="yellow" 
                on:mouseover={() => {
                  hoveredData = student;
                }}
                on:focus={() => {
                  hoveredData = student;
                }}
                tabIndex="0"
                />
      {/each}
      </g>
    </svg>
    {#if hoveredData}
      <Tooltip data={hoveredData} {xScale} {yScale} />
    {/if}
  </div>
  
  <style>
    circle {
      transition: r 300ms ease, opacity 300ms ease;
      cursor: pointer;
    }
  
    circle:focus {
      outline: none;
    }
    .container{
        text-align: center;
    }
    h1{
        color: brown;
        text-align: center;
        margin-bottom: 100px;
    }
  </style>  