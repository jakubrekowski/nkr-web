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
	import { each } from 'svelte/internal';

	let query = `
	query fetch {
		unit(id: "t51ymhoK2vzYFQl5YnrO") {
			number
			model {
				id
				factoryType
				specTable
				manufacturerModel
				intendentUse
				type
				series
				documentation {
					title
					author
					issueNumber
					publisher
					releaseDate
					type
					url
				}
			}
			owner {
				id
				name
			}
			manufacturer {
				name
				shortName
				country
				creationDate
				works
				dateOfLiquidation
			}
			state
			countryOfOperation
			assignments
			repairHistory
		}
	}
	`

	let title = `Naturalna Kolej Rzeczy`

	const response = (async () => {
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
		console.log(response.data)
		title = `${response.data.unit.model.series} ${response.data.unit.number} - Naturalna Kolej Rzeczy`
		return response.data
	})
</script>

<style>
	.content {
		border-radius: 50px;
    }

	.m-main {
		margin-left: 20%;
        margin-right: 20%;
	}

	.bg-primary {
		background: #0C70B9;
	}

	.heroInfo {
		width: 30%;
	}

	.heroImage {
		width: 70%;
		height: 570px;
		border-radius: 50px;
		background: url('/sm30-662.jpg');
		background-position: 0 50%;
		background-size: 100%;
	}

	.tableRow:nth-child(odd) {
		background: #f3f4f6;
	}	

	.book {
		width: 700px;
	}

	.book img {
		height: 250px; 
		width: auto;
		max-width: 250px;
	}

	.specTable div:first-child .rowHead {
		border-radius: 30px 0 0 0;
		padding-top: 10px;
	}

	.specTable div:first-child .rowBody {
		border-radius: 0 30px 0 0;
		padding-top: 10px;
	}

	.specTable div:last-child .rowHead {
		border-radius: 0 0 0 30px;
		padding-bottom: 10px;
	}

	.specTable div:last-child .rowBody {
		border-radius: 0 0 30px 0;
		padding-bottom: 10px;
	}

</style>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<main>
	<Navbar/>
	{#await response}
		<pre>
			Awaiting...
		</pre>
	{:then result}
		<section class="content m-main bg-gray-200 relative">
			<div class="heroInfo p-8 float-left">
				{result.unit.model.type}
				<div class="text-3xl font-bold">{result.unit.model.series} {result.unit.number}</div>
				<div class="mt-4"><span class="font-bold">Producent:</span> {result.unit.manufacturer.name}</div>
				<div><span class="font-bold">Typ:</span> {result.unit.model.factoryType}</div>
				<div><span class="font-bold">Model:</span> {result.unit.model.manufacturerModel}</div>
				<div><span class="font-bold">Przeznaczenie:</span> {result.unit.model.intendentUse}</div>
				<div><span class="font-bold">Stan:</span> {result.unit.state}</div>
				<div class="mt-4"><span class="font-bold">Właściciel:</span> <div>{result.unit.owner.name}</div></div>
				<div class="mt-4"><span class="font-bold">Kraj produkcji:</span> {result.unit.manufacturer.country}</div>
				<div><span class="font-bold">Kraj eksploatacji:</span> {result.unit.countryOfOperation}</div>
			</div>
			<div class="heroImage float-right bg-cover"></div>
			<div class="clear-both"></div>
		</section>

		<section class="mt-8 m-main">
			<div class="w-1/3 float-left">
				<div class="text-2xl font-bold">Specyfikacja techniczna</div>

				<!-- spec table -->
				<div class="mt-4 specTable">
					{#await JSON.parse(result.unit.model.specTable) then raw}
						{#each raw as row}
							{#if row.section}
								<div class="bg-primary text-center text-white font-bold p-1">{row.name}</div>
							{:else}
								<div class="flex justify-between row">
									<div class="rowHead w-1/2 bg-gray-300 pl-3 p-1">{row.head}</div>
									<div class="rowBody w-1/2 bg-gray-100 pl-3 p-1">{row.body}</div>
								</div>
							{/if}
						{/each}
					{:catch err}
						err
					{/await}
				</div>

			</div>
			<div class="w-2/3 float-left pl-8">
				{#if result.unit.assignments !== ''}
					<div class="text-2xl font-bold">Przydziały</div>

					<div id="table" class="odd:bg-gray-100 mt-4">
						<div class="tableRow p-1 flex font-bold">
							<div style="width: 100px">Data</div>
							<div style="width: 50%">Lokomotywownia</div>
							<div>Przewoźnik</div>
						</div>

						{#await JSON.parse(result.unit.assignments) then raw}
							{#each raw as row}
								{#if row.red}
									<div class="tableRow p-1 text-red-600 flex">
										<div style="width: 100px">{row.date}</div>
										<div style="width: 50%">{row.depot}</div>
										<div>{row.carrier}</div>
									</div>
								{:else}
									<div class="tableRow p-1 flex">
										<div style="width: 100px">{row.date}</div>
										<div style="width: 50%">{row.depot}</div>
										<div>{row.carrier}</div>
									</div>
								{/if}
							{/each}
						{:catch err}
							Błąd wczytywania historii.
						{/await}
					</div>
				{/if}

				{#if result.unit.repairHistory !== ''}
					<div class="text-2xl font-bold mt-6">Historia napraw i malowań</div>

					<div id="table" class="odd:bg-gray-100 mt-4">
						<div class="tableRow p-1 flex font-bold">
							<div style="width: 120px">Data</div>
							<div style="width: 25%">Przedsiębiorstwo</div>
							<div style="width: 35%">Malowanie</div>
							<div>Typ naprawy</div>
						</div>

						{#await JSON.parse(result.unit.repairHistory) then raw}
							{#each raw as row}
								<div class="tableRow p-1 flex">
									<div style="width: 120px">{row.date}</div>
									<div style="width: 25%">{row.place}</div>
									<div style="width: 30%">{row.skin}</div>
									<div>{row.type}</div>
								</div>
							{/each}
						{:catch err}
							Błąd wczytywania historii.
						{/await}
					</div>
				{/if}

			</div>
			<div class="clear-both"></div>
		</section>

		<section class="mt-8 m-main">
			<div class="text-2xl font-bold mb-4"><a name="dokumentacja">Dokumentacja</a></div>
			{#each result.unit.model.documentation as doc}
				<div class="book flex bg-gray-100 rounded-3xl mb-4">
					<img class="rounded-3xl rounded-bl-3xl" alt src="https://i.imgur.com/o1odsRP.jpg">
					<div class="p-4 relative">
						<div class="text-xl">{doc.title}</div>
						<div><span class="font-bold">Autor:</span> {doc.author}</div>
						<div><span class="font-bold">Wydanie:</span> {doc.issueNumber}</div>
						<div><span class="font-bold">Wydawca:</span> {doc.publisher}</div>
						<div><span class="font-bold">Data wydania:</span> {doc.releaseDate}</div>

						<!-- <div class="absolute bottom-16 text-xs text-gray-400">Ta książka nie została jeszcze uwolniona, i nie jest dostępna online.</div> -->
						<a href="{doc.url}" class="absolute bottom-4 rounded-full px-4 py-2 mt-6 block w-min text-white bg-blue-600 whitespace-nowrap flex items-center hover:bg-blue-800 transition-all" target="_blank" rel="no-referrer">
							{#if doc.type === 'liblary'}
								<span class="material-icons mr-2">
									local_library
								</span>
								<span>Znajdź bibliotekę</span>
							{:else}
								<span class="material-icons mr-2">
									auto_stories
								</span>
								<span>Przeczytaj online</span>
							{/if}
							
						</a>
					</div>
				</div>
			{/each}

		</section>

		<section class="mt-8 m-main">
			<div class="text-2xl font-bold mb-4">Galeria</div>
			<div class="grid grid-cols-4">
				<div class="bg-green-400 rounded-xl" style="width: 100%; height: 300px; background-position: contain; background: url('http://kskwroclaw.pl/wp-content/uploads/2021/01/131907796_725816308366268_4729925706838544515_n.jpg')">.</div>
				
			</div>
		</section>
	{:catch error}
		<div class="p-4 mx-16 bg-gray-100 text-pink-600 rounded font-mono">
			{error.toString()}
		</div>
	{/await}
	<Footer />
</main>