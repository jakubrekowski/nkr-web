<script>
    export let data;
</script>

<style>
    .text-gray-light {
        color: #6D6D6D;
    }

    .text-gray-secondary {
        color: #A8A8A8;
    }

    .text-primary {
        color: #0C70B9;
    }

    img {
        width: 100px;
        height: 100px;
        border-radius: 30px;
    }

    a {
        width: 700px;
        height: 100px;
    }

    .result {
        display: inline-block;
        position: relative;
        left: 50%;
        transform: translateX(-50%);
    }
</style>

<div class="result">
    <a class="flex flex-row items-center" href="/unit/{data.unit}">
        <img class="block" src={data.image} alt="">
        <div class="ml-3 resultDescription">
            <div class="text-gray-light">{data.owner}</div>
            <div class="text-primary text-2xl">{data.name}</div>
            <div class="text-gray-secondary">{data.model}</div>
        </div>
    </a>
</div>