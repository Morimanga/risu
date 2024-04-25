<script>
    export let readerTotalPages;
    export let readerMangaName;
    export let readerChapter;

    let currentPage = 1;
    let totalPages = readerTotalPages;

    import { onMount } from "svelte";

    onMount(() => {
        const mangaImage = document.getElementById("reader_image");
        const pageSelector = document.getElementById("reader_pages-selector");

        // skeleton loading of the manga image
        mangaImage.onload = () => mangaImage.classList.remove("skeleton-block");

        function readerPage(page) {
            if (!page) return new Error("No `page` provided!");

            if (isNaN(page))
                return new TypeError(
                    `\`page\` must be a number, received "${typeof page}"`,
                );

            if (page > totalPages) return;
            if (page < 0) return;

            mangaImage.setAttribute("data-page", page);
            pageSelector.value = page;

            mangaImage.src = `/images/${readerMangaName}/chapter-${readerChapter}/${page}.jpg`;
        }

        function previousPage() {
            if (currentPage > 1) {
                currentPage--;
                readerPage(currentPage);
            }
        }

        function nextPage() {
            if (currentPage < totalPages) {
                currentPage++;
                readerPage(currentPage);
            }
        }

        pageSelector.onchange = () => {
            currentPage = pageSelector.value;
            readerPage(currentPage);
        };

        // adding pages to the page selector
        for (let i = 0; i < totalPages; i++) {
            const option = document.createElement("option");
            option.value = i + 1;
            option.innerText = `Страница ${i + 1}`;
            pageSelector.appendChild(option);
        }

        const nextButtons = document.querySelectorAll(
            "[data-readernextbutton]",
        );
        nextButtons.forEach((el) => {
            el.onclick = () => nextPage();
        });

        const prevButtons = document.querySelectorAll(
            "[data-readerprevbutton]",
        );
        prevButtons.forEach((el) => {
            el.onclick = () => previousPage();
        });

        document.onkeydown = (e) => {
            if (e.key === "ArrowRight") nextPage();
            if (e.key === "ArrowLeft") previousPage();

            if (e.key === "d") nextPage();
            if (e.key === "a") previousPage();

            if (e.key === "Enter") nextPage();
        };

        readerPage(currentPage);
    });
</script>

<div class="manga-reader">
    <div class="manga-reader_mobilecontrols">
        <i
            class="fas fa-chevron-left"
            data-readerprevbutton="true"
            role="button"
        ></i>
        <p>Глава {readerChapter}</p>
        <i
            class="fas fa-chevron-right"
            data-readernextbutton="true"
            role="button"
        ></i>
    </div>

    <img
        data-page="1"
        class="manga-reader_image skeleton-block image"
        id="reader_image"
        alt="Now Loading..."
    />

    <div class="select manga-reader_page-selector">
        <select id="reader_pages-selector" autocomplete="off"></select>
    </div>

    <p class="manga-reader_page-selector_button">
        Страница {currentPage} / {totalPages}
    </p>
</div>

<style>
    .manga-reader {
        text-align: center;
    }

    .manga-reader_image {
        margin-top: 10px;
        margin-bottom: 10px;
        width: 95%;
        border-radius: 3px;
    }

    .manga-reader_mobilecontrols {
        display: none;
        margin-top: 1em;
        margin-bottom: 2em;
    }

    .manga-reader_mobilecontrols > p {
        margin-left: 1em;
        margin-right: 1em;
    }

    .manga-reader_mobilecontrols > i {
        cursor: pointer;
        margin-top: 5px;
    }

    /* using Bulma's .select causing that arrow icon break page on mobile */
    .manga-reader_page-selector {
        bottom: 5em;
        left: 5%;
        position: fixed;
    }

    .manga-reader_page-selector_button {
        display: block;
        bottom: 3em;
        left: 5%;
        position: fixed;
        margin-top: 0;
    }

    @media only screen and (max-width: 736px) {
        .image {
            display: inline;
        }

        .manga-reader_mobilecontrols {
            display: flex;
            justify-content: center;
        }

        .manga-reader_page-selector {
            position: static;
        }

        .manga-reader_page-selector_button {
            display: inline;
            position: static;
            margin-left: 1em;
            vertical-align: middle;
        }
    }

    @media only screen and (max-width: 820px) {
        .manga-reader_mobilecontrols {
            display: flex;
            justify-content: center;
        }
    }
</style>
