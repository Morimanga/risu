<script>
    export let name;
    export let image;
    export let type;

    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
</script>

<a class="card" on:click={() => dispatch('click')}>
    <div class="card-image">
        <figure class="card_image-shadow">
            <img
                src={image}
                alt="cover-img"
                class="card-title_cover is-skeleton"
                on:load={(e) => e.target.classList.remove('is-skeleton')}
            />

            <figcaption class="card-image-overlay">
                <p class="card-title_type">{type}</p>
                <p class="card-title_name">
                    {name}
                </p>
            </figcaption>
        </figure>
    </div>
</a>

<style>
    .card-image-overlay {
        position: absolute;
        bottom: 6px;
        left: 0;
        right: 0;
        padding: 10px;
        text-align: left;
        background: linear-gradient(
            to top,
            rgba(0, 0, 0, 0.8) 0%,
            rgba(0, 0, 0, 0) 100%
        );
        color: white;
        border-bottom-left-radius: 10px;
        border-bottom-right-radius: 10px;
        border-bottom: 1px solid #000;
    }

    .card {
        width: 200px !important;
        height: 300px !important;
        cursor: pointer;
    }

    .card-title_cover {
        width: 200px;
        height: 300px;
        background: none;
    }

    .card-title_type {
        color: #e1e1e1;
        font-size: 12px;
    }

    .card-title_name {
        font-size: 14px;
        width: 180px;
        max-height: 60px;
        overflow: hidden;
        word-wrap: normal;
        white-space: break-word;
        text-overflow: ellipsis"...";
    }
</style>
