<script>
	import Header from "./Header.svelte";
	import Philosophy from "./Philosophy.svelte";
	//import SkateLessons from "./SkateLessons.svelte";
	import Tools from "./Tools.svelte";
	import Forum from "./Forum.svelte";
	let page = "forum";
</script>

<main {page}>
	<div class="container">
		<Header bind:page={page}/>
	</div>
	<div class="container">
		{#if page == "philosophy"}<Philosophy/>{/if}
		<!--{#if page == "skateLessons"}<SkateLessons/>{/if}-->
		{#if page == "tools"}<Tools/>{/if}
		{#if page == "forum"}<Forum/>{/if}
	</div>
</main>

<style>
</style>