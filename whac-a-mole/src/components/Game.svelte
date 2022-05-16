<script>
    import { onMount } from "svelte";
    import Hole from "./Hole.svelte";

    let holes = [];
    let state = {

        current: "Lycka till!",
        alive: "Du lever!",
        dead: "Bra försök :("

    }
    
    let score = 0;
    let lives = 3;

    const checkLives = () => {
        if (lives > 0) {
            state.current = state.alive
        } 
        else {
            state.current = state.dead
        }
    }

    const pickEmptyHole = () => {

        let hole;
        
        if (!holes.map(hole => !!hole.content).includes(false)) return null;
        do {
            hole = holes[Math.floor(Math.random() * holes.length)];
        } while (hole.content !== null);
        return hole;
    }

    onMount(() => {
        // Create 9 holes
        [...Array(9)].map((_, i) => {
            holes.push({
                content: null,  // either "mole", "bomb" or null
                // timeoutId: null,
                index: i
            });
        });
        // Trigger re-render
        holes = holes;

        setInterval(() => {
            checkLives()
            if (state.current != state.dead) {
                console.log(1)
                // Randomly select a hole
                const hole = pickEmptyHole();
                
                hole.content = "mole";
                // Trigger re-render
                holes = holes;

                setTimeout(() => {
                    hole.content = null;
                }, 2000);
            };
        }, 2500);

        setTimeout(() => {
                setInterval(() => {
                checkLives()
                if (state.current != state.dead) {

                    // Randomly select a hole
                    const hole = pickEmptyHole();

                    hole.content = "bomb";
                    // Trigger re-render
                    holes = holes;

                    setTimeout(() => {
                        hole.content = null;
                    }, 2000);

                };
                
            }, 2500);

            

        }, 2500*4 + 1000)


    });

    const handleMoleClick = e => {
        console.log(e);
        const {index} = e.detail;
        const hole = holes[index];
        //clearTimeout(hole.timeoutId);

        if (hole.content === "mole") {
            hole.content = null;
            holes = holes;
            score += 1;
        } else if (hole.content === "bomb") {
            hole.content = null;
            lives -= 1;
            holes = holes;
        }
    }
</script>

<style lang="scss">

    @import './src/variables';

    @mixin center{

        display: flex;
        flex-direction: column;
		align-items: center;

    }
    #holes {

        display: inline-grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1rem;

    }

    #front_text {

        @include center;

        h2 {

            color: $gray;

        }
    }

</style>

<section id="front_text">
    <h2>{state.current}</h2>
    <h2>Poäng: {score}, liv: {lives}</h2>
</section>

<section id="holes">
    {#each holes as hole}
        <Hole on:clickHole={handleMoleClick} {hole} />
    {/each}
</section>
