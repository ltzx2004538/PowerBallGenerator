<script>
	import PowerBall from "$components/PowerBall.svelte";

	const options = [
		{ color: 'blue', component: PowerBall }
	];

	let selected = options[0];
</script>

<main>Main Page</main>
<svelte:component this={PowerBall} />