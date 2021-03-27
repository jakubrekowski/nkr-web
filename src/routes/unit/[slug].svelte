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

	let query = `
	query fetch {
		unit(id: "${slug}") {
			number
			model {
				id
				factoryType
				specTable
				manufacturerModel
				intendentUse
				type
				tractionType
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
			images {
				id
				author
				date
				description
				tags {
					id
					name
				}
			}
			heroImage {
				id
			}
			updateDate
		}
	}
	`

	let title = `Naturalna Kolej Rzeczy`
	let imagePreviewLength = 0;
	let date

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
		title = `${response.data.unit.model.series} ${response.data.unit.number} - Naturalna Kolej Rzeczy`
		imagePreviewLength = response.data.unit.images.length
		date = new Date(response.data.unit.updateDate)
		return response.data
	})

	let displayImagePreviwe = 'hidden';
	let imagePreviewIndex = 0;

	function openImagePreview(index) {
		imagePreviewIndex = index
		displayImagePreviwe = '';
	}

	function closeImagePreview() {
		displayImagePreviwe = 'hidden';
	}

	function nextImagePreview() {
		if (imagePreviewIndex === imagePreviewLength - 1) {
			imagePreviewIndex = 0;
			return
		}

		imagePreviewIndex++;
	}

	function previousImagePreview() {
		if (imagePreviewIndex === 0) {
			imagePreviewIndex = imagePreviewLength - 1;
			return
		}

		imagePreviewIndex--;
	}
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
		background-position: 0 50%;
		background-size: 100%;
		object-fit: cover;
	}

	.tableRow:nth-child(odd) {
		background: #f3f4f6;
	}	

	.book {
		width: 100%;
	}

	.book img {
		height: 220px; 
		width: auto;
		max-width: 220px;
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

	.gallery-container {
		display: grid;
		grid-template-columns: 1fr 1fr 1fr 1fr;
		grid-template-rows: 1fr;
		gap: 15px 15px;
	}

	.galleryItem {
		aspect-ratio: 1 / 1;
		background-repeat: no-repeat;
		background-size: auto 102%;
		background-color: #3f3f3f;
		background-position: center;
	}

	.galleryItem:hover {
		background-size: auto 110%;
		cursor: pointer;
	}

	.imagePreview {
		z-index: 10;
		position: fixed;
		top: 0;
		left: 0;
		background: #000;
		width: 100vw;
		height: 100vh;
	}

</style>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<main>
	<Navbar/>
	{#await response}
		<pre>
			Ładowanie torów...
		</pre>
	{:then result}
		<section class="content m-main bg-gray-200 relative">
			<div class="heroInfo p-8 float-left">
				{result.unit.model.type} {result.unit.model.tractionType}
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
			<img class="float-right heroImage" src="http://localhost:8000/img/{result.unit.heroImage.id}" alt="">
			<!-- <div class="  bg-cover" > -->
			<!-- </div> -->
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
								<div class="bg-gray-700 text-center text-white font-bold p-1">{row.name}</div>
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

					<div id="table" class="odd:bg-gray-100 mt-4 mb-8">
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

				<section class="">
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
								<a href="{doc.url}" class="absolute bottom-4 rounded-full px-4 py-2 mt-6 w-min text-white whitespace-nowrap flex items-center bg-primary hover:bg-blue-800 transition-all" target="_blank" rel="no-referrer">
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

			</div>
			<div class="clear-both"></div>
		</section>

		<section class="mt-8 m-main">
			<div class="text-2xl font-bold mb-4">Galeria</div>
			<div class="gallery-container">
				{#each result.unit.images as image, index}
					<div class="rounded-3xl galleryItem transition-all duration-150" style="background-image: url('http://localhost:8000/img/{image.id}')" on:click={() => openImagePreview(index)}>
					</div>
				{/each}
			</div>
		</section>

		<div class="imagePreview flex flex-col items-end justify-end {displayImagePreviwe}">
			<div class="w-full h-full bg-center bg-no-repeat bg-contain relative" style="background-image: url('http://localhost:8000/img/{result.unit.images[imagePreviewIndex].id}')">
				<i class="fas fa-times block absolute top-6 right-6 text-white text-3xl hover:text-gray-300 transition-all cursor-pointer" on:click={closeImagePreview}></i>
				<div class="flex flex-row justify-between items-center h-full text-white text-5xl">
					<div on:click={previousImagePreview}>
						<i class="fas fa-chevron-left ml-6 hover:text-gray-300 transition-all cursor-pointer"></i>
					</div>
					<div on:click={nextImagePreview}>
						<i class="fas fa-chevron-right mr-6 hover:text-gray-300 transition-all cursor-pointer"></i>
					</div>
				</div>
			</div>
			<div class="description px-6 py-3 bg-white text-xl w-full">
				{result.unit.images[imagePreviewIndex].description}
			</div>
			<div class="imageFoot px-6 py-2 text-sm bg-gray-200 text-gray-800 w-full flex cursor-default">
				<div>
					<i class="fas fa-user mr-2"></i> {result.unit.images[imagePreviewIndex].author}
				</div>
				<div class="ml-8">
					<i class="fas fa-calendar-week mr-2"></i> {result.unit.images[imagePreviewIndex].date}
				</div>
				<div class="ml-auto cursor-pointer" on:click={() => {
					window.location.href = `/support/report/image?${result.unit.images[imagePreviewIndex].id}`
				}}>
					<i class="fas fa-flag mr-2"></i> Zgłoś nadużycie
				</div>
				<div class="ml-8">
					<a href="https://creativecommons.org/share-your-work/public-domain/" target="_blank" class="block hover:text-gray-600 transition-all duration-150">
						<i class="fab fa-creative-commons-pd mr-2"></i> Domena Publiczna
					</a>
				</div>
			</div>
		</div>

		<div class="text-center my-8 text-sm text-gray-500">
			Ostatnia edycja {date.getDate()}/{#if date.getMonth() < 9}0{/if}{date.getMonth() + 1}/{date.getFullYear()}
		</div>
	{:catch error}
		<div class="p-4 mx-16 bg-gray-100 text-pink-600 rounded font-mono">
			{error.toString()}
		</div>
	{/await}
	<Footer />
</main>