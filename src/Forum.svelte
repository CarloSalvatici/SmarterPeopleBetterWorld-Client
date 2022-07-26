<script>
	import axios from 'axios';
	import { onMount } from 'svelte';
	import Thought from './Thought.svelte';

	//const forumUrl = 'api/content/'
	//const accountsUrl = 'api/accounts/'
	const forumUrl = 'http://localhost:5000/api/content'
	const accountsUrl = "http://localhost:5000/api/accounts"

	// Data for all of the posts
	let forumEntries = [{thoughtID:"0",approvalRating:{totalAverage:0,totalWeight:0,ratings:[]},author:"SmarterPeopleBetterWorld",description:"so, the posts are supposed to be loading, and if you're seeing this for too long then there's probably something wrong",subThoughts:[]}]
	// Data for making a new post
	let newPostData = {}
	// Account Data
	let accountData = {}
	let inputUsername = ""
	let inputPassword = ""
	let loginMsg = ""
	// Variables for the make post
	let makePost = "Make Post"
	let postDescription = ""
	let makeLabel2 = ""
	let makeInput1 = ""
	let makeInput2 = ""

	//Calling backend
	onMount(async () => {
		try {
			let res = await axios.get(forumUrl, {
				headers: {},
				params: {},
			})
			forumEntries = res.data
			console.log("Response in Forum.svelte")
			console.log(res)
		} catch (err) {
			console.log(err);
		}
	})
	async function getForumData() {
		try {let res = await axios.get(forumUrl, {
				headers: {},
				params: {},
			})
			forumEntries = res.data
			console.log(res)} catch (err) {
			console.log(err);
		}
	}
	//Functions for making new posts
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
		getForumData()
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
	//Functions for accounts
	async function makeNewAccount() {
		let newAccountData = {username: inputUsername, password: inputPassword}
		try {let res = await axios.post(accountsUrl, newAccountData)
			loginMsg = res.data.loginMsg
			accountData = res.data.accountData
			console.log(res)} catch (err) {
			console.log(err);
		}
	}
	async function login() {
		try {let res = await axios.get(accountsUrl, {
				headers: {},
				params: {
					username: inputUsername,
					password: inputPassword
				},
			})
			loginMsg = res.data.loginMsg
			accountData = res.data.accountData
			console.log(res)} catch (err) {
			console.log(err);
		}
	}

</script>

<main>
	<div class="container">
		<h3>Forum!</h3>
		<hr/>
		<div class="container">
			<input placeholder="Username" bind:value={inputUsername}/>
			<input type="password" placeholder="Password" bind:value={inputPassword}/>
			<button class="mx-1" on:click={login}>Login</button><button class="mx-1" on:click={makeNewAccount}>Make New Account</button>
			<span>{loginMsg}</span>
		</div>

		{#if Object.keys(accountData).length != 0}
			<button on:click={makePostFunc}>{makePost}</button>
			{#if makePost == "Collapse"}
				<div>Description</div><input bind:value={postDescription}>
				<br/>
				<button on:click={postData}>Post</button>
		{/if} {/if}

		<br>
		<div class = "posts-container">
			{#each forumEntries as data}
				<div class="post">
					<Thought thoughtData={data}/>
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