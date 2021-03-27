<script context="module">
  let slug
	
	export async function preload({ params }) {
    slug = params.slug
	}
</script>

<script>
	import Navbar from '../../components/Navbar.svelte';
	import Footer from '../../components/Footer.svelte';

	import fetch from 'node-fetch';

  let title = 'Stowarzyszenie'

  let query = `
	query fetch {
		owner(id: ${slug}) {
      id
      name
    } 
	}
	`

  const data = (async () => {
		const response = await fetch('http://localhost:4000', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				'Accept': 'application/json',
			},
			body: JSON.stringify({query: query})
		})
		return await response.json()
	})().then((response) =>{
		return response.data
	})

</script>

<style>
	.m-main {
		margin-left: 20%;
		margin-right: 20%;
	}
</style>

<svelte:head>
	<title>{title} - Naturalna Kolej Rzeczy</title>
</svelte:head>

<main>
	<Navbar/>
  <div class="m-main markdown">
    {#await data}
			Ładownie torów...
		{:then content}
      .
		{/await}
  </div>
	<Footer/>
</main>