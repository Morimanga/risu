<script>
    import Header from "../../../components/Header.svelte";
    import NoAddonSelected from "../../../components/NoAddonSelected.svelte";

    import { page } from "$app/stores";
    import { onMount } from "svelte";

    const titleId = $page.params.id;

    let title = [];
    let titleGenres = [];
    let titleReleaseDate;

    let isAddonSelected = false;

    onMount(async () => {
        const selectedAddon = localStorage.getItem("selectedAddon");

        if (selectedAddon) {
            isAddonSelected = true;

            const req = await fetch(
                `http://localhost:8080/addons/get-comics?remote_id=${titleId}&addon_id=${selectedAddon}`,
            );

            const res = await req.json();

            title = res;
            titleGenres = res.genres;

            // const releaseDate = res.chapters.pop().release_date;

            // const releaseDay = new Date(releaseDate).getDay();
            // const releaseMonth = new Date(releaseDate).getMonth();
            // const releaseYear = new Date(releaseDate).getFullYear();

            // titleReleaseDate = `${releaseDay} ${releaseMonth} ${releaseYear}`;

            titleReleaseDate = res.chapters.pop().release_date;
        }
    });
</script>

<Header />

<div class="container">
    {#if isAddonSelected}
        <article class="media">
            <figure class="media-left has-text-center">
                <p class="image">
                    <img
                        src={title.poster}
                        class="title_image is-skeleton"
                        alt={title.name}
                        on:load={(e) =>
                            e.target.classList.remove("is-skeleton")}
                    />
                </p>

                <a href="/title/{titleId}/view">
                    <button
                        class="button is-link mt-3 mb-3 title_readbutton"
                        style="width: 100%; border-radius: 15px;"
                        >Читать с первого тома</button
                    >
                </a>

                <p class="has-text-left m-1">Жанры:</p>
                <div class="fixed-grid is-row-gap-3 has-text-left">
                    {#each titleGenres as genre}
                        <span
                            class="cell tag is-success m-1 has-text-centered"
                            style="width: fit-content;"
                        >
                            {genre}
                        </span>
                    {/each}
                </div>
            </figure>
            <div class="media-content">
                <div class="content">
                    <p class="title_media-info">
                        <i class="fas fa-eye"></i>

                        <span style="font-weight: 900;">1440</span>
                        <span class="ml-2">{titleReleaseDate}</span>
                    </p>

                    <h1 class="title_name">
                        title.localizedName
                        <span class="title_name-divider">/</span>
                        <span class="title_name-originalname">
                            {title?.name}
                        </span>
                    </h1>

                    <p class="title_desc mt-2">{title?.description}</p>
                </div>
            </div>
        </article>
    {:else}
        <NoAddonSelected />
    {/if}
</div>

<style>
    .container {
        display: flex;
        background-color: #24292e;
        margin-top: 2em;
        border-radius: 15px;
        padding: 8px;
        width: 1024px;
    }

    .title_image {
        border-radius: 15px;
        width: 220px;
        height: 256px;
    }

    .title_name {
        margin: 0 !important;
        font-size: 25.6px;
        font-weight: 600;
        color: #f2f2f2;
        width: 600px;
    }

    .title_name-divider {
        color: #555;
    }

    .title_name-originalname {
        color: #999;
        font-weight: normal;
    }

    .title_media-info {
        font-size: 11px;
        color: #c3c3c3;
        text-transform: uppercase;
    }

    .title_desc {
        width: 440px;
        font-size: 16px;
        color: #eee;
    }

    @media only screen and (max-width: 736px) {
        .container {
            width: 380px !important;
        }

        .title_image,
        .media-left {
            width: 120px;
            margin-right: 10px;
        }

        .title_name {
            font-size: 20px !important;
            width: 240px !important;
        }

        .title_desc {
            font-size: 13px !important;
            width: 220px !important;
        }

        .title_readbutton {
            font-size: 12.5px;
            border-radius: 15px !important;
            white-space: unset;
        }
    }

    @media only screen and (max-width: 820px) {
        .container {
            width: 780px;
        }

        .title_name {
            width: 450px;
        }
    }

    @media only screen and (max-width: 375px) {
        .container {
            width: 350px !important;
        }

        .title_name {
            font-size: 20px !important;
            width: 220px !important;
        }

        .title_desc {
            font-size: 12px !important;
            width: 205px !important;
        }
    }

    @media only screen and (max-width: 320px) {
        .container {
            width: 300px !important;
            margin-top: 1em;
            margin-bottom: 1em;
        }

        .title_name {
            font-size: 18px !important;
            width: 180px !important;
        }

        .title_desc {
            font-size: 12px !important;
            width: 160px !important;
        }
    }
</style>
