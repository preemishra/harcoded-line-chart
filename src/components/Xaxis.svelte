<script>
  export let height;
  export let xScale;
  export let margin;

  let xTicks = xScale.ticks(10);
</script>
<g>
  {#each xTicks as tick}
    <text
      x={xScale(tick)}
      y={height - margin.bottom}
      dy="6"
      dominant-baseline="hanging">{tick}% </text
    >
    
  {/each}
  <text></text>
</g>

