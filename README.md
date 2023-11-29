# create-svelte

Everything you need to build a Svelte project, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/master/packages/create-svelte).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm create svelte@latest

# create a new project in my-app
npm create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.

## Setup Inlang i18n
```bash
npx @inlang/paraglide-js@latest init
npm i
```

```json
package.json

"scripts": {
    "compile": "paraglide-js compile --project ./project.inlang.json"
}
```

Create a [messages folder](./messages) and add translation; `se.json` and `en.json` files.

```json
se.json

{
	"$schema": "https://inlang.com/schema/inlang-message-format",
	"currentLanguageTag": "Den aktivarade språktaggen är \"{languageTag}\".",
	"greeting": "Välkommen {name}! Du har {count} meddelanden."
}
```

```json
en.json

{
	"$schema": "https://inlang.com/schema/inlang-message-format",
	"currentLanguageTag": "The current language tag is \"{languageTag}\".",
	"greeting": "Welcome {name}! You have {count} messages."
}
```

In the [routes folder](/src/routes) create a [lang folder](/src/routes/[lang]) and add a [+page.svelte](/src/routes/[lang]/+page.svelte) file. 

```svelte
/src/routes/[lang]/+page.svelte

<script lang="ts">
    import { setLanguageTag, languageTag } from "../../paraglide/runtime";
    import * as m from "../../paraglide/messages"
</script>
    
<p>{m.greeting({ name: "Samuel", count: 5 })}</p>
<p>{m.currentLanguageTag({ languageTag: languageTag() })}</p>
<p>{m.header()}</p>
<button on:click={() => setLanguageTag("se")}>change language to "se"</button>
<button on:click={() => setLanguageTag("en")}>change language to "en"</button>
```

In the [routes folder](/src/routes) add a [+layout.svelte](/src/routes/+layout.svelte) file. 

```svelte
/src/routes/+layout.svelte

<script lang="ts">
    import ParaglideJsSvelteKitAdapter from "../lib/ParaglideSvelteKit.svelte"
</script>

<ParaglideJsSvelteKitAdapter>
    <slot></slot>
</ParaglideJsSvelteKitAdapter>
```

In the [lib folder](/src/routes) create a component named [ParaglideSvelteKit.svelte](/src/lib/ParaglideSvelteKit.svelte).

```svelte
/src/lib/ParaglideSvelteKit.svelte

<script lang="ts">
    import { setLanguageTag, onSetLanguageTag, languageTag } from '../paraglide/runtime';
    import { getContext, setContext } from "svelte"
    import { page } from '$app/stores'
      
    setContext("languageTag", $page.params.lang)
  
    setLanguageTag(() => getContext("languageTag"))
    let oldLanguageTag = languageTag();

    if (import.meta.env.SSR === false){
      onSetLanguageTag((newLanguageTag) => {
        let location = $page.url.pathname.split('/').slice(2).join('/')
        // Does nothing if you attempt to change the language to the current language
        if (newLanguageTag === oldLanguageTag) return;

        window.location.href = `/${newLanguageTag}/${location}`
        
      });
    }

</script>

<slot></slot>
```

In the [routes folder](/src/routes) add a [+page.server.ts](/src/routes/+page.server.ts) file. This file will redirect users from the startpage to a `[lang]` slug page. 

```ts
/src/routes/+page.ts

import { redirect } from '@sveltejs/kit';
 
export function load() {
  throw redirect(308, '/se');
}
```

Finally compile your project.

```bash
npm run compile
```

This will output all required files to the [/src/paraglide folder](/src/paraglide/)