<script>
    import { onMount } from 'svelte';
    export let index;

    let points = [];
    const spacing = 12;
    let radius = 4;

    onMount(() => {
        const width = window.innerWidth;
        const height = window.innerHeight;
        const pointsPerSideX = 70;
        const pointsPerSideY = 30;
        const totalGeneratedPoints = 1309;

        // Assuming a fixed number of points for simplicity
        for (let i = 0; i < totalGeneratedPoints; i++) {
            const x = i % pointsPerSideX;
            const y = Math.floor(i / pointsPerSideX);
            points.push({
                x: x * spacing + radius,
                y: y * spacing + radius,
                isGreen: i < 339 + 161,
                // Increment delay for each point, adjusting the multiplier as needed
                animationDelay: i * 5 // Adjust this value for the desired effect
            });
        }
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
        {#each points as point, i}
        <circle cx={point.x} cy={point.y} r="4" 
            class:point={point.isGreen} 
            style="animation-delay: {point.animationDelay}ms;" 
            fill="black" />
        {/each}
    {/if}
</svg>

<style>
    /* Apply the animation only to points that should be green */
    .point {
        fill: black; /* Default color */
        animation: turnGreen 0.5s forwards; /* Adjust timing as needed */
    }

    /* Keyframes to animate the fill color */
    @keyframes turnGreen {
        to {
            fill: green;
        }
    }

    .graph{
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

    /* Other styles remain unchanged */
    .graph.visible {
        margin: none;
        opacity: 1;
        visibility: visible;
        padding:0%;
    }
</style>




  