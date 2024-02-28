<script>
    import { onMount } from 'svelte';
    export let index;

    let points = [];
    const spacing = 12; // Adjust based on the size of the square and SVG area
    let radius = 4; // Example radius

    onMount(() => {
        const width = window.innerWidth;
        const height = window.innerHeight;
        const pointsPerSideX = Math.floor((width - radius * 2) / spacing);
        console.log( Math.floor((width - 4 * 2) / spacing));
        const pointsPerSideY = Math.floor((height - radius * 2) / spacing);
        console.log( Math.floor((width - 4 * 2) / spacing));
        const totalGeneratedPoints = pointsPerSideX * pointsPerSideY;

        // Correctly calculate the half point of the generated points
        const halfPoint = 750;

        for (let x = 0; x < 30; x++) {
            for (let y = 0; y < 50; y++) {
                // Add each point with the isGreen property set based on its index
                points.push({ x: x * spacing + radius, y: y * spacing + radius, isGreen: points.length < halfPoint, animationDelay: index * 10 });
            }
        }
        // console.log(points.length);
        // console.log(halfPoint);
    });

    let isVisible = false;
    $: if (index === 4) {
        isVisible = true;
    } else {
        isVisible = false;
    }
</script>

<svg class="graph" width="100vw" height="100vh" class:visible={isVisible}>
    {#if index === 4}
        {#each points as point}
        <circle cx={point.x} cy={point.y} r="4" class="point" style="animation-delay: {point.animationDelay}ms;" fill={point.isGreen ? 'green' : 'black'} />
        {/each}
    {/if}
</svg>

<style>
    .graph {
        width: 100%;
        height: 100vh;
        margin: auto;
        margin-top: 150px;
        opacity: 0;
        visibility: hidden;
        transition: opacity 0.5s, visibility 0.5s;
        justify-content: center;
        align-items: center;
    }
    .graph.visible {
        opacity: 1;
        visibility: visible;
    }
</style>


  