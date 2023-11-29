<script lang="ts">
    import { setLanguageTag, languageTag } from "../../../../paraglide/runtime";
    import * as m from "../../../../paraglide/messages"
</script>
    
<h1>SUBPAGE 2</h1>
<p>{m.greeting({ name: "Samuel", count: 5 })}</p>
<p>{m.currentLanguageTag({ languageTag: languageTag() })}</p>
<p>{m.header()}</p>
<button on:click={() => setLanguageTag("se")}>change language to "se"</button>
<button on:click={() => setLanguageTag("en")}>change language to "en"</button>