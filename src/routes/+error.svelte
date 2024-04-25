<script>
    import { page } from "$app/stores";
</script>

<div class="container" style="text-align: center;">
    <img src="/favicon.png" class="mt-6 is-128x128" alt="Morimanga" style="border-radius: 15px;" />

    <h1 class="mt-5">{$page.status}: {$page.error.message}</h1>
    <a href="/">Вернуться домой</a>
</div>
