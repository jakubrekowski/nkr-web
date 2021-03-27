<script>
	import Navbar from '../../components/Navbar.svelte';
	import Footer from '../../components/Footer.svelte';
	import Map from '../../components/Map.svelte';

	import fetch from 'node-fetch';

  let query = `
	query fetch {
		associations {
      id
      name
			adress
			geopoint
    } 
	}
	`
	let markers = []

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
		markers = response.data.associations

		return response.data
	})

</script>

<style>
	.result {
		width: 700px;
	}

	.text-gray-secondary {
		color: #A8A8A8;
	}

	.text-primary {
		color: #0C70B9;
	}

	.result:hover .text-primary {
		color: #08558b;
	}
</style>

<svelte:head>
	<title>Stowarzyszenia - Naturalna Kolej Rzeczy</title>

	<script defer src="https://maps.googleapis.com/maps/api/js?key=AIzaSyAco_oVbcJP9h9eynajv5qwZv1u7oDpNag&map_ids=27ec8f13ee381840"></script>
</svelte:head>

<main>
	<Navbar/>
  <div class="">
    {#await data}
			Ładownie torów...
		{:then content}
			<Map markers={markers}></Map>
      {#each content.associations as association}
        <div>
          <div class="result relative left-1/2 transform -translate-x-1/2">
            <a class="flex flex-row items-center" href="/associations/{association.id}">
              <!-- <img class="block" src={data.image} alt=""> -->
              <div class="ml-3 resultDescription">
                <div class="text-primary text-2xl transition-all duration-150 ">{association.name}</div>
                <div class="text-gray-secondary">{association.adress}</div>
              </div>
            </a>
        </div>
        </div>
      {/each}

		{/await}
  </div>
	<Footer/>
</main>