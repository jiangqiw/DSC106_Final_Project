<script>
  import Scroller from "@sveltejs/svelte-scroller";
  import Map from "./Map.svelte";
  import { geoMercator } from "d3-geo";
  import Graph from "./Graph.svelte";
  import GenderPointsPage from "./gender_points_page.svelte";

  let count, index, offset, progress;
  let width, height;

  // let adjustedWidth = width * 1;
  // let adjustedHeight = height * 1;

  let geoJsonToFit = {
    type: "FeatureCollection",
    features: [ 
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [1, 0],
        },
      },
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [0, 1],
        },
      },
    ],
  };

  $: projection = geoMercator().fitSize([width * 1.05, height * 1.05], geoJsonToFit);
</script>

<style>
  header {
    width: 100%;
    text-align: center;
    font-size: 2em; 
    margin: 1em 0;
    outline: none;
  }

  /* .background {
    width: 100%;
    height: 100vh;
    outline: none;
  }

  .background::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: url('titanic.jpeg');
    background-size: cover;
    background-position: center;
    opacity: 0.3; }  */
    /* z-index: -1; */

    .background {
      width: 100%;
      height: 100vh;
      outline: none;
      position: relative; /* This should ensure that z-index is properly applied to children */
    }

    .background::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-image: url('titanic.jpeg');
      background-size: cover;
      background-position: center;
      opacity: 0.3;
      z-index: -1; /* This should be uncommented to ensure it stays behind the Map component */
    }

  .foreground {
    width: 50%;
    margin: auto;
    height: auto;
    position: relative;
    /* outline: rgb(134, 130, 130) solid 3px; */
    margin-left: auto;
    margin-right: auto; 
  }

  .progress-bars {
    position: absolute;
    background: rgba(169, 168, 169, 0.2) /*  40% opaque */;
    visibility: visible;
  }

  section {
    height: 80vh;
    /* background-color: rgba(0, 0, 0, 0.2); 20% opaque */
    background-color: none;
    /* color: white; */
    /* outline: rgb(196, 193, 196) solid 3px; */
    outline: none;
    text-align: center;
    max-width: 750px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }

</style>

<header>Titanic Whatsoever</header>

<Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <div class="background" slot="background" bind:clientWidth={width} bind:clientHeight={height}>
    <Map bind:geoJsonToFit {index} />
    <Graph {index} {width} {height} {projection} />
    <GenderPointsPage {index} />
    <div class="progress-bars">
      <p>total progress:<strong>{index + 1}/{count}</strong></p>
      <progress value={progress || 0} />
    </div>
  </div>

  <div class="foreground" slot="foreground">
    <section>This is the first section.</section>
    <section>This is the second section.</section>
    <section>This is the third section.</section>
    <section>This is the fourth section.</section>
    <section>This is the fifth section.
    </section>
    <section>This is the sixth section.</section>
  </div>

</Scroller>