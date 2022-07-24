<script>
	import axios from 'axios';
	import { onMount } from 'svelte';

	//const url = 'api/forum/'
	const forumUrl = 'http://localhost:5000/api/forum'
	let forumEntries = [{title:"Loading Posts", description:"so, the posts are supposed to be loading, and if you're seeing this for too long then there's probably something wrong"}]
	let newPostData = {}
	let makePost = "Make Post"
	let postTitle = ""
	let postAuthor = ""
	let postDescription = ""

	//Calling backend
	onMount(async () => {
		try {
			let res = await axios.get(forumUrl, {
				headers: {},
				params: {},
			})
			forumEntries = res.data
			console.log(res)
		} catch (err) {
			console.log(err);
		}
	})
	async function getData() {
		try {
			let res = await axios.get(forumUrl, {
				headers: {},
				params: {},
			})
			forumEntries = res.data
			console.log(res)
		} catch (err) {
			console.log(err);
		}
	}
	async function postData() {
		newPostData = {
			account: "whatever",
			title: postTitle,
			description: postDescription
		}
		postTitle = ""
		postDescription = ""
		try {
			let res = await axios.post(forumUrl, newPostData)
			console.log(res)
		} catch (err) {
			console.log(err);
		}
		getData()
	}
	const makePostFunc = () => {
		if (makePost == "Make Post") {
			makePost = "Collapse"
			return
		}
		if (makePost == "Collapse") {
			makePost = "Make Post"
			return
		}
	}

</script>

<main>
	<div class="container">
		<h3>Forum!</h3>
		<hr/>
		<button on:click={makePostFunc}>{makePost}</button>
		{#if makePost == "Collapse"}
			<div>Title</div><input bind:value={postTitle}><span>{postTitle}</span>
			<div>Description</div><input bind:value={postDescription}>
			<br/>
			<button on:click={postData}>Post</button>
		{/if}
		<br>
		<button on:click={getData}>Call Api!</button>
		<div class = "posts-container">
			{#each forumEntries as data}
				<div class="post">
					<h4>{data.title}</h4>
					<div>{data.description}</div>
				</div>
			{/each}
		</div>
	</div>
</main>

<style>
  .posts-container {
	margin: 5px;
  }
  .post {
	margin: 5px 0px;
  }
</style>