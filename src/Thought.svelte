<script>
    export let thoughtData;
    import { onMount } from 'svelte';

    import Thought from "./Thought.svelte"
    onMount(() => {
        console.log("thoughtData.subThoughts in Thought.svelte")
		console.log(thoughtData.subThoughts)
        console.log("thoughtData in Thought.svelte")
        console.log(thoughtData)
	})
    //function updateThoughtData() {
    //    tempData = thoughtData
    //    thoughtData = tempData
    //    return
    //}
</script>

<main>
	<div class="thought">
        <h6>{thoughtData.author}</h6>
        <div>{thoughtData.description}</div>
        {#if thoughtData.subThoughts != undefined}
            {#if thoughtData.subThoughts.length > 0}
                {#each thoughtData.subThoughts as subThought}
                    <Thought thoughtData={subThought}/>
                {/each}
            {/if}
        {/if}
	</div>
</main>

<style>
    .thought {
        margin: 5px;
    }
</style>