<script lang="ts">
  import * as d3 from "d3-geo";
  import * as GeoJSON from "geojson";
  import * as topojson from "topojson-client";
  import type { Topology } from "topojson-specification";

  import timezoneTopoJson from "./assets/timezones.json";

  let { timezone = $bindable(null) }: { timezone: string | null } = $props();

  type PolygonFeature = GeoJSON.Feature<GeoJSON.Polygon, GeoJSON.GeoJsonProperties>;

  function createTimeZonePolygonFeatures(): PolygonFeature[] {
    // Read world map for timezones.
    // See https://github.com/evansiroky/timezone-boundary-builder
    //     https://github.com/topojson/topojson
    const tzData = timezoneTopoJson as unknown as Topology;
    const tzDataFeature = topojson.feature(tzData, tzData.objects.timezones);
    const features = (tzDataFeature as { features: PolygonFeature[] }).features;
    return features;
  }

  function onclick(e: MouseEvent) {
    if (!(e.target instanceof Element)) return;
    if (e.target.id) {
      timezone = e.target.id;
    }
  }

  function onkeydown(e: KeyboardEvent) {
    if (!(e.target instanceof Element)) return;
    if (e.key !== "Enter") return;
    if (e.target.id) {
      timezone = e.target.id;
    }
  }

  const pathGenerator = d3.geoPath();
  const timeZonePolygonFeatures = createTimeZonePolygonFeatures();
  const tzPaths = $derived(
    timeZonePolygonFeatures.map((d: PolygonFeature) => {
      const id = `${d.properties!.id}`;
      const generatedPath = pathGenerator(d);

      return { id, generatedPath };
    })
  );
</script>

<svg viewBox="0 0 800 320">
  <g style="cursor: pointer" transform="matrix(2 0 0 -2 400 200)">
    {#each tzPaths as { id, generatedPath } (id)}
      <path
        role="button"
        class:selected={id === timezone}
        tabindex="0"
        {id}
        data-testid={id}
        d={generatedPath}
        stroke-width="0.5"
        {onclick}
        {onkeydown}
      >
        <title>{id}</title>
      </path>
    {/each}
  </g>
</svg>

<style>
  @layer defaults {
    path {
      opacity: 0.4;
      stroke: lightgrey;
      fill: lightgrey;
    }
    .selected {
      opacity: 1;
      stroke: darkgrey;
      fill: darkgrey;
    }
  }
</style>
