<script>
    import Header from "../../components/Header.svelte";
    import AddonCard from "../../components/AddonCard.svelte";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let searchQuery;

    onMount(
        () => (searchQuery = new URLSearchParams(location.search).get("q")),
    );

    const cover = "/images/addons/addon-mangalib.png";

    function selectAddon(addonName) {
        if (!addonName) return console.error("No `addonName` provided!");

        localStorage.setItem('selectedAddon', addonName);

        goto('/search');
    }
</script>

<Header />

<div class="search-container container">
    <h1 class="search-container_title">Поиск аддонов</h1>

    <div class="search-container_search-elements">
        <input
            type="text"
            value={searchQuery}
            class="input search-container_searchinput"
            autocomplete="off"
            placeholder="Поиск..."
        />

        <button class="search-container_searchbutton">
            <img
                src="/images/search.svg"
                class="search-container_searchbutton-icon"
                alt="search-icon"
            />
        </button>
    </div>
</div>

<div
    class="titles-container columns is-multiline is-mobile is-centered fixed-grid has-3-cols has-3-cols-desktop has-1-cols-mobile has-2-cols-tablet"
    style="margin-top: 3em;"
>
    <div class="title-cards_container grid">
        <AddonCard
            name="MangaLib"
            desc="Огромная база манги в рунете, здесь вы можете прочитать мангу, манхву, маньхуа на русском."
            image={cover}
            on:click={() => selectAddon('mangalib')}
        />

        <AddonCard
            name="MangaLib"
            desc="Огромная база манги в рунете, здесь вы можете прочитать мангу, манхву, маньхуа на русском."
            image={cover}
            on:click={() => selectAddon('0')}
        />

        <AddonCard
            name="MangaLib"
            desc="Огромная база манги в рунете, здесь вы можете прочитать мангу, манхву, маньхуа на русском."
            image={cover}
        />

        <AddonCard
            name="MangaLib"
            desc="Огромная база манги в рунете, здесь вы можете прочитать мангу, манхву, маньхуа на русском."
            image={cover}
        />

        <AddonCard
            name="MangaLib"
            desc="Огромная база манги в рунете, здесь вы можете прочитать мангу, манхву, маньхуа на русском."
            image={cover}
        />
    </div>
</div>

<style>
    .search-container {
        background-color: #24292e;
        margin-top: 3em;
        border-radius: 15px;
        padding: 8px;
        width: 750px;
        height: 150px;
    }

    .search-container_title {
        font-size: 32px;
        font-weight: bold;
        text-align: center;
    }

    .search-container_search-elements {
        display: flex;
        justify-content: center;
        text-align: center;
        margin-top: 1em;
    }

    .search-container_searchinput {
        width: 250px;
        height: 40px;
        border: 2px solid #31363f;
        border-radius: 15px 0px 0px 15px;
        background-color: #1c2b2d;
        box-shadow: none;
    }

    .search-container_searchbutton {
        border-radius: 0px 15px 15px 0px;
        background-color: #31363f;
        height: 40px;
        width: 50px;
    }

    .search-container_searchbutton-icon {
        width: 24px;
        vertical-align: top;
    }

    @media only screen and (max-width: 736px) {
        .search-container {
            width: 380px;
        }
    }

    @media only screen and (max-width: 375px) {
        .search-container {
            width: 350px;
        }
    }

    @media only screen and (max-width: 320px) {
        .search-container {
            width: 300px;
            height: fit-content;
        }

        .search-container_title {
            font-size: 24px;
        }
    }
</style>
