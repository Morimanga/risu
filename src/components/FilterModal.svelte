<script>
    import { onMount } from "svelte";

    export let onsave;

    onMount(async () => {
        async function saveFilters(filters) {
            if (!filters) return new Error("No `filters` provided!");

            window.postMessage(
                JSON.stringify({
                    type: "filters",
                    years: filters.years,
                    genres: filters.genres,
                }),
            );
        }
    });
</script>

<div class="modal-background" id="filter_modal-background" hidden></div>
<div class="filter-container container" id="filter_modal" hidden>
    <h1 class="filter-container_title">Фильтр</h1>

    <div class="filter-container_filter-entry">
        <h3 class="filter-container_filter-entry_title">Год выпуска</h3>

        <div class="is-flex">
            <input
                class="input filter-container_filter-entry_date-input is-small"
                type="date"
                min="2019-01-01"
                max="2021-12-31"
                id="filter-date_from"
            />

            <hr style="border: 1px solid #444; width: 30px" />

            <input
                class="input filter-container_filter-entry_date-input is-small"
                type="date"
                min="2019-01-01"
                max="2021-12-31"
                id="filter-date_to"
            />
        </div>
    </div>

    <div class="filter-container_filter-entry">
        <h3 class="filter-container_filter-entry_title">Жанр</h3>

        <div class="is-flex">
            <select
                class="input mt-2 filter-container_filter-entry_genre-input"
                autocomplete="off"
                id="filter-genre"
            >
                <option value="etti">Этти</option>
                <option value="shonen">Сёнен</option>
                <option value="dummy">Шоколадка</option>
            </select>
        </div>
    </div>

    <div class="filter-container_filter-entry">
        <h3 class="filter-container_filter-entry_title">Сортировка</h3>

        <div class="is-flex">
            <select
                class="input mt-2 filter-container_filter-entry_genre-input"
                autocomplete="off"
                id="filter-genre"
            >
                <option value="etti">Этти</option>
                <option value="shonen">Сёнен</option>
                <option value="dummy">Шоколадка</option>
            </select>
        </div>
    </div>

    <button
        class="button filter-container_filter_save-button"
        on:click={() => onsave({})}>Сохранить</button
    >
</div>

<style>
    .filter-container {
        background-color: #191c1c;
        border: 2.5px solid #444;
        border-radius: 15px;
        padding: 16px;
        width: 450px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: 50;
        text-align: center;
    }

    .filter-container_title {
        display: block;
        margin: auto;
        background-color: #222;
        border-radius: 15px;
        width: 140px;
        font-size: 32px;
        font-weight: bold;
        border: 2.5px solid #444;
        margin-top: 10px;
    }

    .filter-container_filter-entry {
        margin-top: 2em;
        background-color: #222;
        border: 2.5px solid #444;
        border-radius: 15px;
        width: 380px;
        padding: 8px;
        text-align: left;
    }

    .filter-container_filter-entry_title {
        font-weight: bold;
        font-size: 16px;
    }

    .filter-container_filter-entry_date-input {
        width: 150px;
        margin: 1em;
        border-radius: 10px;
    }

    .filter-container_filter-entry_input {
        border-radius: 10px;
        width: 350px;
    }

    .filter-container_filter-entry_genre-input {
        border-radius: 10px;
        width: 150px;
    }

    .filter-container_filter_save-button {
        background-color: #222;
        width: 200px;
        border-radius: 15px;
        border: none;
        margin-top: 4em;
        font-weight: bold;
        font-size: 16px;
    }

    .modal-background {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: 49;
        background: rgba(0, 0, 0, 0.9);
        width: 100%;
        height: 100%;
    }

    @media only screen and (max-width: 736px) {
        .filter-container {
            width: 385px !important;
            padding: 4px;
        }

        .filter-container_title {
            margin-bottom: 1em;
        }

        .filter-container_filter-entry {
            display: block;
            margin: 20px auto;
            width: 350px;
        }

        .filter-container_filter_save-button {
            margin-bottom: 1em;
        }
    }

    @media only screen and (max-width: 375px) {
        .filter-container {
            width: 345px !important;
            padding: 4px;
        }

        .filter-container_title {
            margin-bottom: 1em;
        }

        .filter-container_filter-entry {
            display: block;
            margin: 20px auto;
            width: 320px;
        }

        .filter-container_filter_save-button {
            margin-bottom: 1em;
        }
    }

    @media only screen and (max-width: 320px) {
        .filter-container {
            width: 300px !important;
            padding: 4px;
        }

        .filter-container_title {
            margin-bottom: 1em;
        }

        .filter-container_filter-entry {
            display: block;
            margin: 20px auto;
            width: 280px;
        }

        .filter-container_filter_save-button {
            margin-bottom: 1em;
        }
    }
</style>
