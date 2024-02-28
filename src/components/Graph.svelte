<script>
  import { fly, draw } from "svelte/transition";
  import { tweened } from "svelte/motion";
  import { cubicOut, cubicInOut } from "svelte/easing";
  import { cities } from "../data/cities";


  export let index, width, height, projection;

  const tweenOptions = {
    delay: 0,
    duration: 1000,
    easing: cubicOut,
  };

  cities.features.forEach((city) => {
  console.log(city.properties.city + ': ' + city.geometry.coordinates);
  });

  const tweenedX = tweened(
    cities.features.map((city) => projection(city.geometry.coordinates)[0]),
    tweenOptions
  );

  const tweenedY = tweened(
    cities.features.map((city) => projection(city.geometry.coordinates)[1]),
    tweenOptions
  );

  $: tweenedData = cities.features.map((city, i) => ({
    x: $tweenedX[i],
    y: $tweenedY[i],
    properties: city.properties,
  }));

  $: {
    if (index === 1) {
      tweenedX.set(cities.features.map((city) => width / 2));
      tweenedY.set(cities.features.map((city, i) => height / 2 + i * 20));
    }

    if ( index === 2) {
      tweenedX.set(
        cities.features.map((city) => projection(city.geometry.coordinates)[0])
      );
      tweenedY.set(
        cities.features.map((city) => projection(city.geometry.coordinates)[1])
      );
    }

    if ( index > 2) {
      tweenedX.set(
        cities.features.map((city) => 0)
      );
      tweenedY.set(
        cities.features.map((city) => 0)
      );
    }


  }


</script>

<svg class="graph">
    {#each tweenedData as city, i}
      {#if city.x && city.y}
        <text
          x={city.x}
          y={city.y}
          id={city.properties.name}
          in:fly={{ x: -300, duration: 200 * i }}
          out:fly={{ x: -300, duration: 200 * i }}
          >{city.properties.city}
        </text>
      {/if}
    {/each}


</svg>

<style>
    .graph {
      width: 100%;
      height: 100vh; /* check problem when setting width */
      position: absolute;
      outline: none;
    }
  </style>