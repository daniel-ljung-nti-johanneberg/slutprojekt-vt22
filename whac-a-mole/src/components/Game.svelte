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
                timeoutId: null,
                index: i
            });
        });
        // Trigger re-render
        holes = holes;

        setInterval(() => {
            checkLives()
            if (lives > 0) {
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
                if (lives > 0) {

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
        clearTimeout(hole.timeoutId);

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
    #holes {
        display: inline-grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1rem;
    }

    div {

        text-align: center;
    }
</style>

<div>
    {state.current}
    Poäng: {score}, liv: {lives}
</div>
<section id="holes">
    {#each holes as hole}
        <Hole on:clickHole={handleMoleClick} {hole} />
    {/each}
</section>
