<script>
	import Navbar from './Navbar.svelte';
	import SearchBar from './SearchBar.svelte';
	import SearchResult from './SearchResult.svelte';
	import Footer from './Footer.svelte';

	let data = [
		{
			image: '/sm30-195.jpg',
			owner: 'PKP Polskie Linie Kolejowe S.A.',
			name: 'SM30 195: Manewrowa Lokomotywa Spalinowa',
			model: 'Fablok Ls300'
    	},{
			image: '/sm30-195.jpg',
			owner: 'Klub Sympatyków Kolei we Wrocławiu',
			name: 'SM30 662: Manewrowa Lokomotywa Spalinowa',
			model: 'Fablok Ls300'
		},{
			image: '/sm30-195.jpg',
			owner: 'Klub Sympatyków Kolei we Wrocławiu',
			name: 'SM30 662: Manewrowa Lokomotywa Spalinowa',
			model: 'Fablok Ls300'
		},{
			image: '/sm30-195.jpg',
			owner: 'Klub Sympatyków Kolei we Wrocławiu',
			name: 'SM30 662: Manewrowa Lokomotywa Spalinowa',
			model: 'Fablok Ls300'
		},{
			image: '/sm30-195.jpg',
			owner: 'Klub Sympatyków Kolei we Wrocławiu',
			name: 'SM30 662: Manewrowa Lokomotywa Spalinowa',
			model: 'Fablok Ls300'
		}
	]
</script>

<style>
	
</style>

<main>
	<Navbar/>
	<SearchBar/>
	<div class="results">
		{#each data as result}
			<div class="mb-6">
				<SearchResult data={result}/>
			</div>
		{/each}
	</div>
	<Footer/>
</main>