<script>
    import { langs } from "../langs.js"
    let content = langs[0].content
</script>

<style>
    .text-gray-light {
        color: #6D6D6D;
    }

    .brand {
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
    }

    footer {
        padding-left: 100px;
        padding-right: 100px;
        color: #A8A8A8;
        font-size: 14px;
        position: relative;
    }

    a {
        transition: all 200ms;
    }

    a:hover {
        color: #525252;
    }

</style>

<footer class="flex flex-row justify-between py-4 mt-8">
    <div class="flex flex-row">
        <div class="text-gray-light">
            &copy; 2021 {content.footer.by} <a href="https://unoff.pl/" target="_blank">unoff.pl</a>
        </div>
        <div class="ml-12">
            <a href="#" class="mr-3">{content.footer.contentSharing}</a>
            <a href="#" class="mr-3">{content.footer.listOfAuthors}</a>
            <a href="#" class="mr-3">{content.footer.contact}</a>
        </div>
    </div>
    <div class="brand">
        <a href="https://unoff.pl/" target="_blank"><img src="/img/un-logo.svg" alt=""></a>
    </div>
    <div>
        <!-- <a href="#" class="ml-3">{content.footer.status}</a> -->
        <!-- <a href="#" class="ml-3">API</a> -->
        <a href="#" class="ml-3">{content.footer.store}</a>
        <a href="#" class="ml-3">{content.footer.blog}</a>
        <a href="#" class="ml-3">{content.footer.about}</a>
    </div>
</footer>