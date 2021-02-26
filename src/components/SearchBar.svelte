<script>
    import Filter from './Filter.svelte'

    import { langs } from "../langs.js"

    let content = langs[0].content
</script>

<style>
    .searchInput {
        width: 595px;
        height: 50px;
        border-radius: 25px 0 0 25px;
        padding-left: 15px;
        padding-right: 15px;
        background-color: #F4F4F4;
    }

    .searchInput:focus {
        outline: none;
    }

    .searchBtn {
        width: 100px;
        height: 50px;
        border-radius: 25px;
        background: #A43BAD;
        color: #fff;
        position: relative;
        left: -25px;
        top: 3px;
        transition: all 200ms;
    }

    .searchBtn:hover {
        background: #7d2d85;
    }
    
    .searchBtn span {
        position: relative;
        top: 30%;
        transform: translateY(-50%);
    }

    .searchBar {
        position: relative;
        left: 50%;
        transform: translateX(-50%);
    }

    .w700{
        width: 700px;
        position: relative;
        left: 50%;
        transform: translateX(-50%);
    }
</style>

<div class="m-16">
    <div class="text-3xl w700">{content.search.header}</div>
    <form class="mt-4">
        <div class="inline-block searchBar">
            <input class="searchInput" type="text" placeholder={content.search.inputPlaceholder}>
            <button class="searchBtn">
                <span class="material-icons">
                    search
                </span>
            </button>
        </div>
    </form>
</div>