<script>
  export let width;
  export let yScale;
  export let margin;
  export let yPath;

  let yTicks = yScale.ticks(5);
</script>

<g transform="translate({margin.left} {margin.top})">
  <path stroke="currentColor" d={yPath} fill="none" />
  {#each yTicks as tick}
    <text x={0} y={yScale(tick)} dy="-6">{tick}</text>
    <line
      x1="0"
      y1={yScale(tick)}
      x2={width}
      y2={yScale(tick)}
      stroke={tick == 0 ? "black" : "#cecece"}
    />
  {/each}
</g>
