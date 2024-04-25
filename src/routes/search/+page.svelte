<script>
    import Header from "../../components/Header.svelte";
    import TitleCard from "../../components/TitleCard.svelte";
    import FilterModal from "../../components/FilterModal.svelte";
    import NoAddonSelected from "../../components/NoAddonSelected.svelte";

    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    const cover = "https://cdn.myanimelist.net/images/anime/1758/141268.jpg";

    let searchQuery;
    let addonSelected = false;
    let cards = [];
    let totalPages = 1;

    let filterYears;
    let filterGenres;

    let getComicsExternal;


    onMount(async () => {
        searchQuery = new URLSearchParams(location.search).get("q");
        addonSelected = localStorage.getItem("selectedAddon");

        async function getComics(page = 1) {
            if (addonSelected) {
                const req = await fetch(
                    `http://localhost:8080/addons/get-latest-comics?page=${page}&sort=true&addon_id=${addonSelected}`,
                );

                const comicsData = await req.json();

                cards = comicsData.content; // Збереження списку карток з отриманих даних
                totalPages = comicsData.all_page;
            }
        }

        getComics();

        // for accessing this function in Svelte's context
        getComicsExternal = getComics;

        // listening for messages from filter modal
        window.onmessage = (message) => {
            const rawData = message.data;
            let data;

            try {
                data = JSON.parse(rawData);
            } catch (e) {
                throw new Error('[search-container_searchbutton:onclick/window.onmessage/parseJSONMessage]: failed to parse JSON');
            }

            if (data.type === 'filters') {
                
            }
        }
    });

    async function openFilterModal() {
        const filterModal = document.getElementById("filter_modal");
        const filterModalBackground = document.getElementById(
            "filter_modal-background",
        );

        filterModalBackground.hidden = false;
        filterModal.hidden = false;
    }

    async function closeFilterModal() {
        const filterModal = document.getElementById("filter_modal");
        const filterModalBackground = document.getElementById(
            "filter_modal-background",
        );

        filterModal.hidden = true;
        filterModalBackground.hidden = true;
    }

    async function searchTitle() {
        const input = document.getElementById("search_input");

        if (!input.value) return;

        const req = await fetch(
            `http://localhost:8080/addons/search-comics?page=${page}&sort=true&addon_id=${addonSelected}&name=${input.value}&years=[]&genres=[]`,
        );

        const comicsData = await req.json();

        cards = comicsData.content; // Збереження списку карток з отриманих даних
        totalPages = comicsData.all_page;
    }


</script>

<Header />
<FilterModal onsave={closeFilterModal} />

{#if addonSelected}
    <div class="search-container container">
        <h1 class="search-container_title">Поиск</h1>

        <div class="search-container_search-elements">
            <input
                type="text"
                id="search_input"
                value={searchQuery}
                class="input search-container_searchinput"
                autocomplete="off"
                placeholder="Поиск..."
            />

            <button
                class="search-container_searchbutton"
                on:click={searchTitle}
            >
                <img
                    src="/images/search.svg"
                    class="search-container_searchbutton-icon"
                    alt="search-icon"
                />
            </button>
        </div>

        <button
            class="ml-6 mt-5 has-text-weight-bold button is-dark"
            on:click={openFilterModal}
        >
            <i class="fas fa-filter" style="margin-right: 7px;"></i>
            Фильтр
        </button>
    </div>

    <div
        class="titles-container columns is-multiline is-mobile is-centered fixed-grid has-3-cols has-3-cols-desktop has-1-cols-mobile has-2-cols-tablet"
        style="margin-top: 3em;"
    >
        <div class="title-cards_container grid">
            {#each cards as card}
                <TitleCard
                    name={card.name}
                    type="Аниме"
                    image={cover}
                    on:click={() => goto(`/title/${card.remote_id}`)}
                />
            {/each}
        </div>
    </div>

    <div
        class="pagination is-centered mt-6"
        role="navigation"
        aria-label="pagination"
    >
        <ul class="pagination-list">
            {#each { length: totalPages } as _, i}
                <li>
                    <button
                        class="pagination-link"
                        on:click={(e) => {
                            getComicsExternal(i + 1);

                            // removing previous active link
                            document
                                .querySelectorAll(".pagination-link.is-current")
                                .forEach((link) =>
                                    link.classList.remove("is-current"),
                                );

                            e.target.classList.add("is-current");
                        }}>{i + 1}</button
                    >
                </li>
            {/each}
        </ul>
    </div>
{:else}
    <NoAddonSelected />
{/if}

<style>
    .search-container {
        background-color: #24292e;
        margin-top: 3em;
        border-radius: 15px;
        padding: 8px;
        width: 750px;
        height: 200px;
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

    .pagination-list li {
        flex-grow: 0;
        margin-top: 10px;
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
        }
    }
</style>
