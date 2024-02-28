<script>
    import { onMount } from 'svelte';
    export let index;
  
    let points = [];
    const totalPoints = 1500;
    const pointsPerSide = Math.ceil(Math.sqrt(totalPoints));
    const spacing = 15; // Adjust based on the size of the square and SVG area
  
    onMount(() => {
      for (let x = 0; x < pointsPerSide; x++) {
        for (let y = 0; y < pointsPerSide; y++) {
          if (points.length < totalPoints) {
            points.push({ x: x * spacing, y: y * spacing });
          }
        }
      }
    });
    
    let isVisible = false;
    $: if (index === 4) {
        isVisible = true;
    } else {
        isVisible = false;
    }
</script>

<!-- Use class directive to conditionally apply the 'visible' class based on `isVisible` -->
<svg class="graph" width="100%" height="100vh" class:visible={isVisible}>
    {#if index === 4}
        {#each points as point}
        <circle cx={point.x} cy={point.y} r="1" fill="black" />
        {/each}
    {/if}
</svg>

<style>
    .graph {
        width: 100%;
        height: 100vh;
        position: absolute;
        top: 0;
        left: 0;
        opacity: 0;
        visibility: hidden;
        transition: opacity 0.5s, visibility 0.5s;
    }
    .graph.visible {
        opacity: 1;
        visibility: visible;
    }
</style>

  