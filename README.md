# svelte-timezone-map-select

Ported from [the react dialog version](https://gitlab.com/kmiyashita/react-timezone-map-select)

## Features

This is a svelte component which lets you select a timezone on world map.

## Installation

`svelte-timezone-map-select` is available as an [npm package](https://www.npmjs.com/package/react-timezone-map-select).

## Usage

```svelte
<script lang="ts">
  import { TimeZoneSelect } from "svelte-timezone-map-select";

  let timezone = $state("America/New_York");
</script>

<TimeZoneSelect bind:timezone />
```

## License

svelte-timezone-map-select is available under the MIT License.
