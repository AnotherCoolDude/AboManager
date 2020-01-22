<script>
	export let name;

	import Abolist from './Abolist.svelte';
	import Filternav from './Filternav.svelte';
	import Filtergrid from './Filtergrid.svelte';

	import './_layout.svelte';
</script>

<main class="container">
	<div class="columns is-centered">
		<div class="column is-7">
			<h1>Hello {name}!</h1>
			<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
		</div>
	</div>
	<div class="columns">
		<div class="column">
			<div class="columns is-centered">
				<div class="column is-7">
					<Abolist />
				</div>
				<div class="column is-3">
					<Filtergrid/>
				</div>
			</div>
		</div>
	</div>
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