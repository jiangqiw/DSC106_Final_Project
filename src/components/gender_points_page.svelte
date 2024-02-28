<script>
    import { onMount } from 'svelte';
    export let index;
  
    let points = [];
    const totalPoints = 1500;
    const pointsPerSide = Math.ceil(Math.sqrt(totalPoints));
    const spacing = 12; // Adjust based on the size of the square and SVG area

    let radius = 4; // Example radius

    onMount(() => {
    const width = window.innerWidth;
    const height = window.innerHeight;
    const pointsPerSideX = Math.floor((width - radius * 2) / spacing);
    const pointsPerSideY = Math.floor((height - radius * 2) / spacing);

    for (let x = 0; x < pointsPerSideX; x++) {
        for (let y = 0; y < pointsPerSideY; y++) {
        points.push({ x: x * spacing + radius, y: y * spacing + radius });
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
<svg class="graph" width="100vw" height="100vh" class:visible={isVisible}>
    {#if index === 4}
        {#each points as point}
        <circle cx={point.x} cy={point.y} r="4" fill="black" />
        {/each}
    {/if}
</svg>

<style>
    .graph {
        width: 100%;
        height: 100vh;
        margin: auto;
        margin-top: 150px;
        position: center;
        opacity: 0;
        visibility: hidden;
        transition: opacity 0.5s, visibility 0.5s;
        justify-content: center;
        align-items: center;
    }
    .graph.visible {
        margin: none;
        opacity: 1;
        visibility: visible;
        padding:0%;
    }
</style>

  