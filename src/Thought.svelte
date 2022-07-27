<script>
    export let thoughtData;
    export let replyFunction = () => {};
    import { onMount } from 'svelte';
    import Thought from "./Thought.svelte"
    let description = ""
    onMount(() => {
        console.log("thoughtData.subThoughts in Thought.svelte")
		console.log(thoughtData.subThoughts)
        console.log("thoughtData in Thought.svelte")
        console.log(thoughtData)
	})
</script>

<main>
	<div class="thought">
        <h6>{thoughtData.author}</h6>
        <span>Post ID: {thoughtData.thoughtID}</span>
        <div class="my-3">{thoughtData.description}</div>
        <input placeholder="reply?" bind:value={description}/><button on:click={replyFunction(description, thoughtData.thoughtID)}>Send</button>
        {#if thoughtData.subThoughts != undefined}
            {#if thoughtData.subThoughts.length > 0}
                {#each thoughtData.subThoughts as subThought}
                    <Thought thoughtData={subThought} replyFunction={replyFunction}/>
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