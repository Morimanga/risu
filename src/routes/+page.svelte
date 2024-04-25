<script>
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    import Header from "../components/Header.svelte";
    import TitleCard from "../components/TitleCard.svelte";
    import NoAddonSelected from "../components/NoAddonSelected.svelte";

    // змінна для збереження списку карток
    let cards = [];
    let folders = [];

    let isAddonSelected = false;

    onMount(async () => {
        async function getLatestComics() {
            const defaultAddon = localStorage.getItem("selectedAddon");

            if (defaultAddon) {
                isAddonSelected = true;

                const req = await fetch(
                    `http://localhost:8080/addons/get-latest-comics?page=1&sort=true&addon_id=${defaultAddon}`,
                );

                const comicsData = await req.json();
                cards = comicsData.content; // Збереження списку карток з отриманих даних
            }
        }

        // @TODO: добавить папку через folder/edit-folder и проверить
        async function getFolders() {
            const req = await fetch("http://localhost:8080/folder/all-folders");

            const foldersData = await req.json();
            folders = foldersData.content;
        }

        getLatestComics();
        getFolders();
    });

   
</script>

<Header />

<section class="user-folders">
    <div class="user-folders_container">
        <div class="user-folders_folder user-folders_folder-active">Аниме</div>
        <div class="user-folders_folder folder-color2">Папка-2</div>
        <div class="user-folders_folder folder-color3">Папка-3</div>
        <div class="user-folders_folder folder-color3">Папка-3</div>
        <div class="user-folders_folder folder-color3">Папка-3</div>
        <div class="user-folders_folder folder-color3">Папка-3</div>
        <div class="user-folders_folder folder-color3">Папка-3</div>
        <div class="user-folders_folder folder-color3">Папка-3</div>
    </div>
</section>

{#if isAddonSelected}
    <div
        class="titles-container columns is-multiline is-mobile is-centered fixed-grid has-3-cols has-3-cols-desktop has-1-cols-mobile has-2-cols-tablet"
        style="margin-top: 3em;"
    >
        <div class="title-cards_container grid">
            {#each cards as card}
                <TitleCard
                    name={card.name}
                    image={card.poster}
                    type="Аниме"
                    on:click={() => goto(`/title/${card.remote_id}`)}
                />
            {/each}
        </div>
    </div>
{:else}
    <NoAddonSelected />
{/if}

<style>
    .user-folders {
        display: flex;
        margin: auto;
        background-color: #24292e;
        margin-top: 2em;
        height: 100%;
        width: 30%;
        justify-content: center;
        border-radius: 15px;
        text-align: center;
        vertical-align: bottom;
    }

    .user-folders_container {
        display: flex;
        width: 100%;
        overflow-x: scroll;
        scrollbar-color: #fff transparent;
    }

    .user-folders_folder {
        display: inline-block;
        border-radius: 8px;
        background-color: #1f6e8c;
        width: 100%;
        color: #fff;
        font-weight: bold;
        font-size: 20px;
        height: 35px;
        text-align: center;
        margin: 15px;
        padding: 2px 18px;
        vertical-align: bottom;
        user-select: none;
    }

    .user-folders_folder-active {
        box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
    }

    .folder-color2 {
        background-color: #ff204e;
    }

    .folder-color3 {
        background-color: #1e5128;
    }

    .titles-container {
        margin-top: 3em;
    }

    .error-container {
        margin-top: 3em;
        text-align: center;
    }

    .error-container h1 {
        font-size: 28px;
        font-weight: bold;
    }

    @media only screen and (max-width: 736px) {
        .user-folders {
            width: 95%;
            margin-top: 3em;
        }

        .user-folders_folder {
            margin: 10px;
        }
    }
</style>
