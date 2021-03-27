<script>
	import Navbar from '../../components/Navbar.svelte';
	import Footer from '../../components/Footer.svelte';

  import marked from 'marked'
	import fetch from 'node-fetch';

  const data = (async () => {
		const response = await fetch('http://localhost:3000/markdown/contentSharing.md')
		return await response.text()
	})()

</script>

<style>
	.m-main {
		margin-left: 20%;
		margin-right: 20%;
	}
</style>

<svelte:head>
	<title>Udostępnianie treści - Naturalna Kolej Rzeczy</title>
</svelte:head>

<main>
	<Navbar/>
  <div class="m-main markdown">
    {#await data}
			Ładownie torów
		{:then content} 
      {@html marked(content)}
		{/await}
  </div>
	<Footer/>

  <div class="hidden">
    <h1>.</h1>
    <h2>.</h2>
    <h3>.</h3>
  </div>
</main>