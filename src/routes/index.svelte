<script>
	import Navbar from '../components/Navbar.svelte';
	import SearchBar from '../components/SearchBar.svelte';
	import SearchResult from '../components/SearchResult.svelte';
	import Footer from '../components/Footer.svelte';

	import fetch from 'node-fetch';

	let query = `
	query fetch {
		units {
			id
			number
			model {
				series
				factoryType
				type
				intendentUse
			}
			owner {
				name
			}
			manufacturer {
				shortName
				country
			}
			state
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
		const resData = response.data.units;
		// console.error(resData);
		
		let toPrint = []

		resData.forEach(elem => {
			toPrint.push({
				image: '/sm30-195.jpg',
				owner: elem.owner.name,
				name: `${elem.model.series} ${elem.number}: ${elem.model.type} / ${elem.model.intendentUse}`,
				model: `${elem.manufacturer.shortName} ${elem.model.factoryType}`,
				unit: elem.id
			})
		})

		console.log(typeof toPrint, toPrint)

		return toPrint
	})

	console.log(typeof data, data)
</script>

<style>
	
</style>

<main>
	<Navbar/>
	<SearchBar/>
	<div class="results">
		{#await data}
			Ładownie torów
		{:then list} 
			{#each list as result}
				<div class="mb-6">
					<SearchResult data={result}/>
				</div>
			{/each}
		{/await}
	</div>
	<Footer/>
</main>