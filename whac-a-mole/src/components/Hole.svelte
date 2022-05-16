
<script>
    import Mole from "./Mole.svelte";
    import Bomb from "./Bomb.svelte";

    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let hole;

</script>

<style lang="scss">

    @import './src/variables';

    @mixin center{

        display: flex;
        justify-content: center;
        align-items: center;

    }


    .hole {
        @include center;
        width: 10rem;
        height: 10rem;
        background-color: #ddd;
        border-radius: 1rem;
        box-shadow: $shadow;
    }
</style>

<div class="hole" on:click={() => dispatch("clickHole", {index: hole.index})}>
    {#if hole.content === "mole"}
        <Mole />
    {:else if hole.content === "bomb"}
        <Bomb />
    {/if}
</div>