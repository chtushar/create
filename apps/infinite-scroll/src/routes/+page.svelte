<script lang="ts">
    import { onMount } from 'svelte';
    import '../app.css';
    import { writable } from 'svelte/store';
    import { setContext } from 'svelte';
    import Item from '../lib/Item.svelte';
    import data from '../data.json';

    let container: HTMLDivElement;
    const containerHeight = writable(0);
    const scrollY = writable(0);
    const max = data.length * 100;
    
    setContext("max", max)
    setContext("containerHeight", containerHeight);
    setContext('scrollY', scrollY);

    onMount(() => {
        $containerHeight = container.clientHeight;
        container.addEventListener('wheel', (event: Event) => {
            let deltaY = (event as WheelEvent).deltaY * 0.1;
            if (deltaY > 120) {
                deltaY = 120;
            } else if (deltaY < -120) {
                deltaY = -120;
            }
            scrollY.update((value) => {
                const newValue = value + deltaY;
                if (newValue > max) {
                    return -max;
                };
                if (newValue < -max) {
                    return 0;
                }
                return newValue;
            });
        });
    })
</script>

<main>
    <div bind:this={container} class="container">
        <ul>
            {#each data as item, index}
                <Item {index} {item} />
            {/each}
        </ul>
    </div>
</main>

<style>
    main {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        background: #1f1f1f;
    }

    .container {
        width: 100%;
        max-width: 402px;
        height: 100%;
        max-height: 402px;
        padding: 20px;
        padding-top: 0;
        padding-bottom: 0;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: flex-start;

        border-radius: 16px;
        border: 1px solid #333;
        background: #222;
        box-shadow: 0 1px 2px 0 rgb(0 0 0 / 0.05);
        color: white;
        overflow: hidden;
    }

    ul {
        position: relative;
        width: 100%;
    }
</style>