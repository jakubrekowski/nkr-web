<script>
    import IconButton from "./IconButton.svelte"
    import { langs } from "../langs.js"

    let content = langs[0].content

    const sites = [
        {
            icon: 'home',
            name: 'Strona główna',
            href: '/'
        },{
            icon: 'groups',
            name: 'Stowarzyszenia',
            href: '/associations'
        },{
            icon: 'museum',
            name: 'Muzea',
            href: '/museums'
        },{
            icon: 'loc',
            name: 'Lokomotywy',
            href: '/?type=locomotive',
            custom: true
        },{
            icon: 'wagon',
            name: 'Wagony',
            href: '/?type=wagon',
            custom: true
        },
        // {
        //     icon: 'photo_library',
        //     name: 'Zdjęcia',
        //     href: '/photos'
        // }
    ]
</script>

<style>
    header {
        width: 100%;
        height: 80px;
        padding-left: 100px;
        padding-right: 100px;
    }

    nav {
        height: 80px;
        margin-left: 30px;
    }

    .brand {
        position: relative;
        top: 50%;
        transform: translatey(-50%);
    }
</style>

<header>
    <a href="/" class="block float-left brand text-2xl font-bold">{content.global.name}</a>
    <nav class="float-left">
        {#each sites as site}
            <IconButton icon={site.icon} active name={site.name} href={site.href} custom={site.custom}/>
        {/each}
    </nav>
    <div class="clear-both"></div>
</header>