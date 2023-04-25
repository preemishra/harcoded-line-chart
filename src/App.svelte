<script>
  import dataCopy from "./data/dataCopy";
  import LineChart from "./LineChart.svelte";
  import data from "./data/data.js";
  

</script>

<LineChart {data}/>
<LineChart data={dataCopy}/>

