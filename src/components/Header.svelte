<script>
    export let readerFeaturesEnabled;
    export let readerTitleOriginalName;
    export let readerTitleTranslatedName;
    export let titleId;
    export let readerChapter;

    import { onMount } from "svelte";

    onMount(() => {
        // Get all "navbar-burger" elements
        const navbarBurgers = document.querySelectorAll(".navbar-burger");

        // Add a click event on each of them
        navbarBurgers.forEach((el) => {
            el.addEventListener("click", () => {
                // Get the target from the "data-target" attribute
                const target = document.getElementById(el.dataset.target);

                // Toggle the "is-active" class on both the "navbar-burger" and the "navbar-menu"
                el.classList.toggle("is-active");
                target.classList.toggle("is-active");
            });
        });
    });
</script>

<nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
        <a class="navbar-item" href="/">
            <img src="/favicon.png" class="header_logo" alt="navbar-header" />

            <h1 class="header_title">Morimanga</h1>
        </a>

        <a
            role="button"
            class="navbar-burger"
            aria-label="menu"
            aria-expanded="false"
            data-target="main-navbar"
        >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
        </a>
    </div>

    <div id="main-navbar" class="navbar-menu">
        <div class="navbar-start">
            {#if readerFeaturesEnabled}
                <div class="navbar-item reader_controls">
                    <i
                        class="fas fa-chevron-left"
                        data-readerprevbutton="true"
                        id="reader_pageBack"
                        role="button"
                    ></i>
                    <p>Глава {readerChapter}</p>
                    <i
                        class="fas fa-chevron-right"
                        data-readernextbutton="true"
                        id="reader_pageNext"
                        role="button"
                    ></i>
                </div>

                <a href="/title/{titleId}">
                    <div class="reader-title_info">
                        <p class="reader-title_originalname">
                            {readerTitleOriginalName}
                        </p>
                        <p class="reader-title_translatedname">
                            {readerTitleTranslatedName}
                        </p>
                    </div>
                </a>
            {:else}
                <a href="/" class="navbar-item header-item_primary">Закладки</a>
                <a href="/addons" class="navbar-item">Аддоны</a>
                <a href="/about" class="navbar-item">Про нас</a>
            {/if}
        </div>
    </div>
</nav>

<style>
    .navbar {
        border-radius: 15px;
        background-color: #24292e;
        margin-top: 10px;
        padding-left: 1em;
        padding-right: 1em;
        height: 55px;
        border-radius: 15px;
    }

    .header_title {
        font-size: 24px;
        font-weight: bold;
        margin-left: 10px;
    }

    .header-item_primary {
        color: #fff !important;
    }

    .navbar-item {
        color: #d3d3d3;
    }

    .navbar-menu {
        border-radius: 15px;
        background-color: #24292e;
    }

    .header_logo {
        max-height: 48px;
        width: 42px !important;
        height: 42px !important;
        border-radius: 100%;
        vertical-align: bottom;
    }

    .reader-title_info {
        margin-top: 0.5em;
        margin-left: 1em;
    }

    .reader_controls {
        margin-left: 2em;
    }

    .reader_controls > p {
        margin-left: 1em;
        margin-right: 1em;
    }

    .reader_controls > i {
        cursor: pointer;
        padding: 7px 8px;
        border: 1px solid #444;
        border-radius: 13px;
        line-height: 1;
        width: 32px;
        text-align: center;
        transition: all 300ms;
    }

    .reader_controls > i:hover {
        background-color: #222;
    }

    .reader-title_originalname {
        color: #888;
        font-size: 12px;
    }

    .reader-title_translatedname {
        color: #eee;
        font-size: 14px;
        font-weight: 500;
    }

    @media only screen and (max-width: 736px) {
        .navbar {
            width: 100%;
            border-radius: 0;
            margin: 0;
            padding: 0;
            display: inline-block;
            text-align: center;
        }

        .navbar-menu {
            border-radius: 0;
        }

        .reader-title_info {
            margin: 0 !important;
        }

        .reader_controls > p {
            display: inline;
        }

        .reader_controls {
            display: block;
            margin-bottom: 2em;
        }
    }

    @media only screen and (max-width: 820px) {
        .reader_controls {
            display: none;
        }

        .navbar {
            width: 100%;
            border-radius: 0;
            margin: 0;
            padding: 0;
            display: inline-block;
            text-align: center;
        }

        .navbar-menu {
            border-radius: 0;
        }

        .reader-title_info {
            margin: 0 !important;
        }

        .reader_controls > p {
            display: inline;
            margin-top: 1em;
            margin-bottom: 1em;
        }

        .reader_controls {
            margin-bottom: 2em;
        }
    }

    @media only screen and (min-width: 800px) {
        .navbar {
            margin: 5px;
            border-radius: 12px;
        }
    }
</style>
