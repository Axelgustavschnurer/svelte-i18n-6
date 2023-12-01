<script lang="ts">
    import ParaglideJsSvelteKitAdapter from "../lib/ParaglideSvelteKit.svelte"
    import { setLanguageTag } from "../paraglide/runtime";
</script>

<layout-main>
    <button on:click={() => setLanguageTag("se")}>Svenska</button>
    <button on:click={() => setLanguageTag("en")}>Engelska</button>
    <ParaglideJsSvelteKitAdapter>
        <slot></slot>
    </ParaglideJsSvelteKitAdapter>
</layout-main>

<style>
    @import "/main.css";
</style>