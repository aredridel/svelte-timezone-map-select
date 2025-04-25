<script lang="ts">
  import Map from "../WorldMap.svelte";

  let timezone = $state("");
</script>

<p>
  {timezone}
</p>

<Map bind:timezone />
