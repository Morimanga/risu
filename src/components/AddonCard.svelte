<script>
    export let name;
    export let image;
    export let desc;

    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
</script>

<a class="card" role="button" on:click={() => dispatch("click")}>
    <div class="card-content">
        <div class="media">
            <div class="media-left">
                <figure class="image">
                    <img class="card-image is-skeleton" src={image} alt={name} on:load={(e) => e.target.classList.remove('is-skeleton')} />
                </figure>
            </div>
            <div class="media-content">
                <h1 class="card-addon_name">{name}</h1>
                <p class="card-addon_desc">{desc}</p>
            </div>
        </div>
    </div>
</a>

<style>
    .card {
        width: 370px;
        height: 200px;
        cursor: pointer;
        border: 3px solid rgba(38, 42, 43, 0.25);
        border-radius: 15px;
        box-shadow: none;
    }

    .card-image {
        width: 128px;
        border-radius: 15px;
    }

    .card-addon_name {
        font-size: 26px;
        font-weight: bold;
    }

    .card-addon_desc {
        font-size: 14px;
    }

    @media only screen and (max-width: 736px) {
        .card {
            width: 360px;
        }
    }

    @media only screen and (max-width: 375px) {
        .card {
            width: 330px;
        }
    }

    @media only screen and (max-width: 320px) {
        .card {
            width: 300px;
        }

        .card-addon_name {
            font-size: 22px;
        }

        .card-addon_desc {
            font-size: 13px;
        }

        .card-image {
            width: 96px;
        }
    }
</style>
