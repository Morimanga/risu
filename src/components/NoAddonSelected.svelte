<script>
    import { goto } from "$app/navigation";
</script>

<div class="error-container">
    <h1>Не выбран аддон</h1>
    <p class="mt-5">
        Вы должны выбрать аддон (источник), чтобы смотреть список тайтлов.
    </p>

    <a on:click={() => goto("/addons")}>Выбрать аддон</a>
</div>

<style>
    .error-container {
        margin-top: 3em;
        text-align: center;
    }

    .error-container h1 {
        font-size: 28px;
        font-weight: bold;
    }
</style>
