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
	let accountData = {username: "gayboy99", password: "asshole", accessLevel: 5}
	let inputUsername = ""
	let inputPassword = ""
	let loginMsg = ""
	// Variables for the make post
	let makePost = "Make Post"
	let postDescription = ""

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
	//Functions for making new forum entries
	async function makeNewForumEntry() {
		newPostData = {
			username: accountData.username,
			password: accountData.password,
			description: postDescription,
			forumPostReqType: "new post",
			timestamp: new Date()
		}
		postDescription = ""
		try {
			let res = await axios.post(forumUrl, newPostData)
			console.log(res)
			getForumData()
		} catch (err) {
			console.log(err);
		}
		getForumData()
	}
	const toggleEntryCreator = () => {
		if (makePost == "Make Post") {
			makePost = "Collapse"
			return
		}
		if (makePost == "Collapse") {
			makePost = "Make Post"
			return
		}
	}
	async function replyFunction(description, thoughtID) {
		console.log(description)
		try {
			let res = await axios.put(forumUrl, {username: accountData.username, password: accountData.password, description: description, thoughtID: thoughtID})
			console.log(res)
			getForumData()
		} catch (err) {
			console.log(err);
		}
	}
	//Delete a forum entry
	async function deleteForumEntry(thoughtID) {
		try {let res = await axios.delete(forumUrl, {
				headers: {},
				params: {thoughtID: thoughtID, username: accountData.username, password: accountData.password},
			})
			getForumData()
			console.log(res)} catch (err) {
			console.log(err);
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
		<!--Login Sign Up Stuff-->
		<div class="my-1">
			<input placeholder="Username" bind:value={inputUsername}/>
			<input type="password" placeholder="Password" bind:value={inputPassword}/>
			<button class="mx-1" on:click={login}>Login</button><button class="mx-1" on:click={makeNewAccount}>Make New Account</button>
			<span>{loginMsg}</span>
		</div>
		<hr/>

		<!--Make New Forum Post-->
		{#if Object.keys(accountData).length != 0}
			<div class="mb-2">
				<button on:click={toggleEntryCreator}>{makePost}</button>
				{#if makePost == "Collapse"}
					<div class="my-1">
						<textarea class="new-post-input" placeholder="Say something" bind:value={postDescription}/>
						<br>
						<button class="new-post-button" on:click={makeNewForumEntry}>Post</button>
					</div>
				{/if} 
			</div>
		{/if}

		<!--Forum Entries Display-->
		<div>
			{#each forumEntries as data}
				<div>
					<Thought thoughtData={data} replyFunction={replyFunction}/>
				</div>
				{#if (accountData.accessLevel >= 5)}
					<button on:click={deleteForumEntry(data.thoughtID)}>Delete</button>
				{/if}
			{/each}
		</div>
	</div>
</main>

<style>
	.new-post-input {
		width: 500px;
		height: 53px;
		resize: both;
	}
/*
	.btttn{
		maybe a button class for less important buttons
	}
	.bttn:hover {
		background-color: #c0c0c0;
		transition: all .3s;
	}
*/

</style>