<script lang="ts">
    import { getContext } from "svelte";
    import type { Writable } from "svelte/store";

    export let item: any;
    export let index: number;
    const scrollY = getContext<Writable<number>>("scrollY");
    const max = getContext<number>("max");
    const containerHeight = getContext<Writable<number>>("containerHeight");

    const offset = 100 * index;
    let show = false;
    let value = $scrollY;

    $: {
        value = $scrollY + offset;

        if (value < -1 * max / 2 ) {
            value = value + max;
        }
        if (value > max / 2) {
            value = value - max;
        }

        show = value > -200 && value < $containerHeight + 200;
    }
    $: styles = `transform: translate3d(0, ${value.toFixed(2)}px, 0);`;

</script>

{#if show}
    <li style={styles}>
        <span>
            {index + 1}.
        </span>
        <span>
            {item.name}
        </span>
    </li>
{/if}

<style>
    li {
        position: absolute;
        top: 0;
        left: 0;
        
        list-style: none;
        height: 100px;
        width: 100%;
        padding: 16px;

        display: flex;
        align-items: center;
        justify-content: flex-start;
        gap: 16px;

        background-color: transparent;
        border-top: 1px solid #444;

        font-size: 1.2rem;
        /* transition: transform 0.1s linear; */
    }
</style>