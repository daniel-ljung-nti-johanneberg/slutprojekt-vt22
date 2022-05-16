
<script>
    import Mole from "./Mole.svelte";
    import Bomb from "./Bomb.svelte";

    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    /** @type {{content: "mole" | "bomb" | null, moleSinceActive: number, index: number}} */
    export let hole;

</script>

<style lang="scss">
    .hole {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 10rem;
        height: 10rem;
        background-color: #ddd;
        border-radius: 5px;
    }
</style>

<div class="hole" on:click={() => dispatch("clickHole", {index: hole.index})}>
    {#if hole.content === "mole"}
        <Mole />
    {:else if hole.content === "bomb"}
        <Bomb />
    {/if}
</div>