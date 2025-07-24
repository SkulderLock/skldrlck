<script lang="ts">
	import Router from "svelte-spa-router";
	import Home from "./pages/Home.svelte";
	import Valentine from "./pages/Valentine.svelte";
	import Dice from "./pages/Dice.svelte";

	const routes = {
		"/": Home,
		"/valentine": Valentine,
		"/dice": Dice,
	};
</script>

<nav>
	<a href="#/">Home</a>
	<a href="#/valentine">Valentine</a>
	<a href="#/dice">Dice</a>
</nav>

<main>
	<Router {routes} />
</main>

<style>
	nav {
		margin-bottom: 20px;
	}

	a {
		margin-right: 10px;
		color: #e84141;
		text-decoration: none;
		font-weight: bold;
	}
</style>
