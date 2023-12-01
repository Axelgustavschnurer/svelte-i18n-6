<!-- Redirects you to the correct place when translating -->
<!-- TODO: Add a redirect when no valid language tag  -->

<script lang="ts">
    import { setLanguageTag, onSetLanguageTag, languageTag } from '../paraglide/runtime';
    import { getContext, setContext } from "svelte"
    import { page } from '$app/stores'
      
    setContext("languageTag", $page.params.lang)
    setLanguageTag(() => getContext("languageTag"))

    // Set lang if no lang
    let url = $page.url.pathname.split('/').slice(1)
    console.log(url)

    let oldLanguageTag = languageTag();

    if (import.meta.env.SSR === false){
      onSetLanguageTag((newLanguageTag) => {
        let location = $page.url.pathname.split('/').slice(2).join('/')
        if (newLanguageTag === oldLanguageTag) return; // Does nothing if you attempt to change the language to the current language

        window.location.href = `/${newLanguageTag}/${location}`
        
      });
    }

</script>

<slot></slot>