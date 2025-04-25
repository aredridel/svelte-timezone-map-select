<script lang="ts">
  import Map from "$lib/WorldMap.svelte";

  let timezone = $state("");
</script>

<p>
  {timezone}
</p>

<Map bind:timezone />
