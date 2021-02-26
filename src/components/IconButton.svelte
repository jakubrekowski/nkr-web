<script>
    export let icon, active, name, href, custom
</script>

<style>
    .iconBtn {
        width: 40px;
        height: 40px;
        background: #0C70B9;
        border-radius: 20px;
        color: white;
        text-align: center;
    }

    .iconLink {
        position: relative;
        top: 50%;
        transform: translateY(-50%);
        margin-right: 20px;
    }

    .iconName {
        width: 0;
        height: 40px;
        background: #F4F4F4;
        border-radius: 0 20px 20px 0;
        position: relative;
        left: -20px;
        z-index: -1;
        transition: all 200ms;
    }

    .iconName span {
        display: none;
        position: relative;
        top: 45%;
        transform: translateY(-50%);
        filter: opacity(0);
        transition: all 200ms;
    }

    .iconLink:hover .iconName {
        width: auto;
        padding-left: 30px;
        padding-right: 15px;
        margin-right: -20px;
    }

    .iconLink:hover .iconName span {
        display: block;
        filter: opacity(1);
    }

    .active {
        background: #A43BAD;
    }

    .material-icons {
        font-size: 23px;
        display: block;
        position: relative;
        left: calc(20 - (23 / 2))px;
        top: 48%;
        transform: translateY(-50%);
    }

    .custom-icon {
        font-size: 23px;
        display: block;
        position: absolute;
        left: 50%;
        top: 48%;
        transform: translateX(-50%) translateY(-50%) scale(1.3);
    }
</style>

{#if custom}
    <a href={href} class="iconLink float-left">
        <div class="iconBtn float-left relative">
            <img class="custom-icon" style="width: 23px" src="/img/icons/{icon}.svg" alt="wagon">
        </div>
        <div class="iconName float-left">
            <span>{name}</span>
        </div>
        <div class="clear-both"></div>
    </a>

{:else}
    {#if active}
        <a href={href} class="iconLink float-left">
            <div class="iconBtn active float-left">
                <span class="material-icons">
                    {icon}
                </span>
            </div>
            <div class="iconName float-left">
                <span>{name}</span>
            </div>
            <div class="clear-both"></div>
        </a>
    {:else}
        <a href={href} class="iconLink float-left">
            <div class="iconBtn float-left">
                <span class="material-icons">
                    {icon}
                </span>
            </div>
            <div class="iconName float-left">
                <span>{name}</span>
            </div>
            <div class="clear-both"></div>
        </a>
    {/if}
{/if}



        