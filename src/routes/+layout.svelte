<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />

	<!-- Google tag (gtag.js): Tracking user metrics -->
	<script async src="https://www.googletagmanager.com/gtag/js?id=G-WFVL4NB2BP"></script>
	<script>
	  window.dataLayer = window.dataLayer || [];
	  function gtag(){dataLayer.push(arguments);}
	  gtag('js', new Date());
	
	  gtag('config', 'G-WFVL4NB2BP');
	</script>

</svelte:head>


<main class="bg-[#006241] min-h-screen p-5 text-[#edf5f2]">
	{@render children?.()}
</main>


