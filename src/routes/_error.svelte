<script>
    import { langs } from "../langs.js";
    const lang = langs[0].content;

    import Navbar from '../components/Navbar.svelte';
    import Footer from '../components/Footer.svelte';

    export let status;
    export let error;
    
	const dev = process.env.NODE_ENV === 'development';
</script>

<style>
    #footer {
        position: fixed;
        bottom: 0;
        width: 100%;
    }
</style>

<svelte:head>
	<title>{status} :: Naturalna Kolej Rzeczy</title>
</svelte:head>

<main>
    <Navbar/>

    <div class="mt-16 text-center text-3xl">
        {#if status === 404}
            <div>{lang.errors.notFound}</div>
        {:else}
            <div>{lang.errors.undefined}</div>
        {/if}    
    </div>
    <img class="mt-12 mx-auto block" src="/img/404.svg" alt="">

    
    {#if dev && error.stack}
        <div class="m-8 bg-gray-100 text-pink-600 p-8 rounded-lg">
            <div class="text-2xl mb-4"><span class="font-bold">Status code:</span> {status}</div>
            <pre>{error.stack}</pre>
        </div>
    {/if}
    
    <div id="footer">
        <Footer/>
    </div>
</main>

