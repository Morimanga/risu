<script>
    import Header from "../../../../components/Header.svelte";
    import MangaReader from "../../../../components/MangaReader.svelte";

    import { page } from "$app/stores";
    import { onMount } from "svelte";

    const titleId = $page.params.id;

    let title = [];
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
        }
    });
</script>

<Header
    readerFeaturesEnabled={true}
    readerTitleOriginalName={title.name}
    readerTitleTranslatedName="title.localizedName"
    readerChapter={"1"}
    {titleId}
/>

<div class="container">
    <!-- @TODO: total pages backend -->
    <MangaReader
        readerMangaName={titleId}
        readerChapter={"1"}
        readerTotalPages={"3"}
    />
</div>
