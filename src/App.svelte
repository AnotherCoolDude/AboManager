<script>
	export let name;
	import 'bulma/css/bulma.css';

	import Abolist from './Abolist.svelte';
</script>

<main class="container">
	<div class="columns is-centered">
		<div class="column is-7">
			<h1>Hello {name}!</h1>
		</div>
	</div>
	<div class="columns is-centered">
		<div class="column is-7">
			<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
		</div>
	</div>

	<Abolist />
</main>

<style>

	h1{
		text-align: center;
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	p {
		text-align: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>