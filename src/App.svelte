<script>
	import Navbar from "./Navbar.svelte";
	let fruits = ["apple", "banana", "lemon"]
</script>

<Navbar />

{#each fruits as item}
<h1>fruit : {item}</h1>
{/each}
