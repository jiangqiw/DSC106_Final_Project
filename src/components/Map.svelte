<script>
  import mapboxgl from "mapbox-gl";
  import { onMount } from "svelte";
  export let index;
  export let geoJsonToFit;

  mapboxgl.accessToken =
    "pk.eyJ1IjoieXVoZXRpYW4iLCJhIjoiY2xzaTNodTd5MjJtMjJrbzBtMzI3dTVmbCJ9.vOpitaAAgbbzvPfngV4_Ug";

  let container;
  let map;

  let zoomLevel;
  let pathIndex = 0;
  const path = [
    [-2.12, 51.06], // Southampton
    [-1.7512, 49.83], // Cherbourg
    [-8.72, 51.9150], // Queenstown (Cobh)
    [-39.10, 45.17]  // Example Sunk point, adjust as necessary
  ];

  function updateZoomLevel() {
    const screenWidth = window.innerWidth;
    zoomLevel = screenWidth <= 600 ? 4 : 4; // Adjust these values as needed
  }

  function handleResize() {
    updateZoomLevel();
    map.setZoom(zoomLevel);
  }

  function animateDot() {
    if (pathIndex >= path.length) {
      pathIndex = 0; // Optionally loop or stop the animation
      return;
    }
    const coordinates = path[pathIndex];
    map.getSource('moving-dot').setData({
      type: 'Point',
      coordinates
    });
    pathIndex++;
    setTimeout(animateDot, 2000); // Adjust timing as needed
  }

  function startAnimation() {
    pathIndex = 0; // Reset path index to start from Southampton
    if (map && map.isStyleLoaded()) {
      // Ensure the map and style are fully loaded before attempting to set data
      map.getSource('moving-dot').setData({
        type: 'Point',
        coordinates: path[0] // Southampton coordinates
      });
      animateDot(); // Restart the animation
    }
  }

  onMount(() => {
    updateZoomLevel();
    map = new mapboxgl.Map({
      container,
      style: "mapbox://styles/mapbox/outdoors-v12",
      center: [-25, 50],
      zoom: zoomLevel,
      attributionControl: true, // removes attribution from the bottom of the map
    });

    window.addEventListener("resize", handleResize);

    function hideLabelLayers() {
      const labelLayerIds = map
        .getStyle()
        .layers.filter(
          (layer) =>
            layer.type === "symbol" && /label|text|place/.test(layer.id)
        )
        .map((layer) => layer.id);

      for (const layerId of labelLayerIds) {
        map.setLayoutProperty(layerId, "visibility", "none");
      }
    }

    map.on("load", () => {
      hideLabelLayers();
      updateBounds();
      map.on("zoom", updateBounds);
      map.on("drag", updateBounds);
      map.on("move", updateBounds);
      map.addSource('moving-dot', {
        type: 'geojson',
        data: {
          type: 'Point',
          coordinates: path[0] // Start at the first point
        }
      });
      map.addLayer({
        id: 'dot',
        source: 'moving-dot',
        type: 'circle',
        paint: {
          'circle-radius': 10,
          'circle-color': '#007cbf'
        }
      });
      animateDot();
    });
  });
  
  function updateBounds() {
    const bounds = map.getBounds();
    geoJsonToFit.features[0].geometry.coordinates = [
           bounds._ne.lng,
           bounds._ne.lat,
          ];
    geoJsonToFit.features[1].geometry.coordinates = [
           bounds._sw.lng,
           bounds._sw.lat,
     ];
   }
  let isVisible = false;
  $: if (index === 2) {
    isVisible = true;
    startAnimation();
  } else {
    isVisible = false;
  }
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://api.mapbox.com/mapbox-gl-js/v2.14.0/mapbox-gl.css"
  />
</svelte:head>

<div class="map" class:visible={isVisible} bind:this={container} />

<style>
  
  .map {
    width: 100%;
    height: 100vh; /* check problem when setting width */
    position: absolute;
    opacity: 0;
    visibility: hidden;
    transition: opacity 2s, visibility 2s;
    outline: blue solid 3px;
  }

  .map.visible {
    opacity: 1;
    visibility: visible;
  }
</style>

