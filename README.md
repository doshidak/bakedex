<h1 align="center">
  <img alt="showdex-lib" width="360px" src=".github/showdex-lib.png">
  <br>
  <a href="https://github.com/doshidak/bakedex"><code>bakedex</code></a>
</h1>

<table align="center">
  <thead>
    <tr>
      <th>&nbsp;<a href="https://bake.dex.tize.io">Repository</a> 2024.09.29&nbsp;</th>
      <th>&nbsp;Serving <a href="https://github.com/doshidak/showdex"><code>showdex</code></a> · <a href="https://github.com/doshidak/showdex/releases/tag/v1.2.5">v1.2.5</a>&nbsp;</th>
    </tr>
  </thead>
</table>

<br>

Repository of [**Showdex**](https://smogon.com/forums/threads/showdex-an-auto-updating-damage-calculator-built-into-showdown.3707265/post-9368925) **asset bundles**. This serves as a [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) for the most up-to-date versions of pre-bundled assets that [Showdexes v1.2.4](https://github.com/doshidak/showdex/releases/tag/v1.2.4)+ will periodically update from.

If you're looking for Showdex's source code, this **isn't** it ([**try here**](https://github.com/doshidak/showdex) instead!).

<strong>tl;dr <sub><em>also like the only explanation lmao</em> <sub><em>jk I added some more c:</em></sub></sub></strong>

* basically how we keep data fresh w/out having to build a new version <sub>since we've never gone MIA & stuff <sub>no cap <sub>fr fr</sub></sub> <sup>ong</sup> ¯\\\_(ツ)\_/¯</sub>
* powered by [GitHub Pages](https://pages.github.com)
  - same tech that keeps the ~~spice~~ [Smogon](https://github.com/pkmn/smogon) & [Randoms](https://github.com/pkmn/randbats) sets flowing !!
  - thank u macrohard u da groats <sub>also thank to <strong>pre</strong> for the idea huehuehue \ (•◡•) /</sub>

<br>
<br>

<h1 align="center">
  Developer Zone
</h1>

> [!CAUTION]
> You are about to get in the zone, the developer zone.  
> If you do not wish to get in the zone, the developer zone, please visit [this zone](https://youtube.com/watch?v=9MiP1MJC7EU) instead.

## Developer SparkNotes™

* API-style routes w/ only **GET** requests cause <sup><em>wait</em></sup> [it's all files](/public/v1) <sub><em>??</em> <sub>always has been</sub></sub>
  - so no tokens or oauth2 or any of that shiz
* these files map to a path that *look like* API route endpoints (e.g., `'https://bake.dex.tize.io/v1/buns'`)
  - but *don't be fooled* !! :o
  - they're actually just *extensionless* UTF-8 files (i.e., fancy way of saying text files without the `*.txt` part)
  - they also just contain stringified JSON, prettified & all, so ya nothing cray
  - the <em>official <sup>AF</sup>-looking</em> API `'/v1'` prefix (+ other things LOL) exist for the added street cred <sub>( ͡° ͜ʖ ͡°)</sub>

terminology bout to get confusing real quick so:

* (**asset**) **bundle** contains a bunch of **assets** (i.e., some *stuff*), like Calcdex player titles & Pokémon *sets*[^1]
* **bundle namespace** contains a bunch of **bundles**, all with the same kind of **asset**

[^1]: Pokémon **sets** are internally referred to as ***presets*** within the Showdex codebase to avoid collisions, mostly of the mental variety, with the commonly used `set` keyword. Otherwise, they're referred to as *sets* in any text displayed to the user.

> [!TIP]
> you can think of a **bundle** like a folder containing all of your JavaScript projects, which that itself is inside of a folder or **bundle namespace** called " *Programming Projects* ", which can have other **bundles** for your HTML[^2] <sub>( ͡° ͜ʖ ͡°)</sub> & Python projects idk lmao  
> <sub>haven't invested much into my <em>analogy conjuration</em> skill tree sorry v_v</sub>

[^2]: Hope you liked that "HTML is a programming language" joke ( ͡° ͜ʖ( ͡° ͜ʖ ͡°)ʖ ͡°)

* **bundle catalog** is that link I mentioned earlier, ya kno the one with the `'/v1/buns'` ??
  - contains a list of all available **bundle namespaces** along with the info about the **bundles** they contain, such as what the **bundles** are called, what kind of **assets** they contain & when they were last updated
  - they might call this the *package* or *release manifest* / *catalog* / *index* in the biz
  - but idk I just whipped this together real quicc

* ayy lmao

> [!TIP]
> can't be bothered & wanna see how Showdex does it?  
> here's where all the :･ﾟ✧  ***magic*** ✧ﾟ･: is! &rarr; [`@showdex/utils/app/bakeBakedexBundles()`](https://github.com/doshidak/showdex/tree/master/src/utils/app/bakeBakedexBundles.ts) <sub>ya nerd <sub>jkjk (｡◕‿◕｡)</sub></sub>  
> <sub><strong>warning:</strong> little (& terrible) documention in that one sorry v_v glhf tho</sub>

### Types

> [!CAUTION]
> ah put your type charts away it's not that kind of [*type*](https://pkmn.help) LOL
> * it's the **boring** [`type`](https://typescriptlang.org)
> * but gotta know what we're dealing with!

#### `BakedexApiResponse`

* every API endpoint / JSON file in this repo will have this basic skeleton
* you can conveniently do a sanity check by checking if we're `ok`
* the `ntt` (i.e., the *en*-*ti*-*ty* or *entity* <sub>get it hehehe</sub>) tells you what's in the `payload`
* the goodies you're looking for will be in `payload`

```ts
// note: simplified; not actual type definition
export interface BakedexApiResponse {
  ok: true; // quicc sanity check (e.g., const data = await response.json(); if (!data.ok) return;)
  status: [code: 200, label: 'OK']; // all cool APIs have this; feel free to ignore
  ntt: 'buns' | 'presets' | 'tiers' | 'titles'; // the entity, or what's inside `payload`
  payload: unknown; // the good stuff, based on what `ntt` is
}
```

> <sup><strong>Source:</strong> <a href="https://github.com/doshidak/showdex/tree/master/src/interfaces/api/BakedexApiResponse.ts"><code>@showdex/interfaces/api/BakedexApiResponse</code></a></sup>

#### `ShowdexAssetBundle`

* remember how **bundle catalogs** contain information about the **bundles** in a **bundle namespace**?
* ya here's what that information on a **bundle** would actually look like, more or less
* this is the basic skeleton, but more props may exist depending on the `ntt`

```ts
// note: this isn't just used for strictly assets hosted here, so extraneous props may exist;
// this is a base skeleton used for describing other stuff, like translation files!
export interface ShowdexAssetBundle {
  id: string; // bundle's id
  ext?: string; // file extension, which doesn't exist here; will be `null` most likely!
  ntt: 'buns' | 'presets' | 'tiers' | 'titles'; // entity
  name: string; // bundle's name
  label?: string; // what users will see in the UI if specified; dw about this tho
  author?: string; // bundle's author(s) -- it's a just string, so no format enforcement here lol
  desc?: string; // optional description
  created: string; // ISO 8601 timestamp
  updated: string; // ISO 8601 timestamp
  disabled: false; // bundle's killswitch; will always be `false` here, but exists cause all the cool kids do it
}
```

> <sup><strong>Source:</strong> <a href="https://github.com/doshidak/showdex/tree/master/src/interfaces/app/ShowdexAssetBundle.ts"><code>@showdex/interfaces/app/ShowdexAssetBundle</code></a></sup>

## API Reference

> [!IMPORTANT]
> * **Base URL:** `'https://bake.dex.tize.io'`
>   - **&rarr; A:** `'https://doshidak.github.io/bakedex'`

### GET [`/v1/buns`](/public/v1/buns)

* **Accept:** `'text/plain'`

<sup><strong>Suggested Request Headers</strong></sup>

```ts
// note: simplified; not actual type definition
export type BakedexApiBunsResponse = BakedexApiResponse & {
  ntt: 'buns';
  payload: {
    // info about the 'players' namespace of Calcdex player title bundles
    players: {
      // info about a particular asset (e.g., 'titles' here)
      // you'll want to hang onto the `id` if you wanna grab the actual asset!!
      [id: string]: ShowdexAssetBundle & {
        ntt: 'titles';
      };
    };

    // info about the 'presets' namespace of Pokemon set bundles
    presets: {
      [id: string]: ShowdexAssetBundle & {
        ntt: 'presets';
        gen: GenerationNum; // -> 1 | 2 | 3 | ... | 8 | 9 (i.e., number enum)
        format: string; // genless; e.g., 'randombattle', 'ou', 'vgc3005'
      };
    };

    // info about the 'supporters' namespace, containing a bundle of the real ones
    supporters: {
      [id: string]: ShowdexAssetBundle & {
        ntt: 'tiers';
      };
    };
  };
};
```

> <sup><strong>Source:</strong> <a href="https://github.com/doshidak/showdex/tree/master/src/interfaces/api/BakedexApiBunsResponse.ts"><code>@showdex/interfaces/api/BakedexApiBunsResponse</code></a></sup>

<br>

### GET [`/v1/players/:id`](/public/v1/players)

* **Accept:** `'text/plain'`

<sup><strong>Suggested Request Headers</strong></sup>

```ts
// note: simplified; not actual type definition
export type BakedexApiTitlesResponse = BakedexApiBunsResponse & {
  ntt: 'titles';
  payload: ShowdexPlayerTitle[];
};
```

> <sup><strong>Source:</strong> <a href="https://github.com/doshidak/showdex/tree/master/src/interfaces/api/BakedexApiTitlesResponse.ts"><code>@showdex/interfaces/api/BakedexApiTitlesResponse</code></a></sup>  
> [`@showdex/interfaces/app/ShowdexPlayerTitle`](https://github.com/doshidak/showdex/tree/master/src/interfaces/app/ShowdexPlayerTitle.ts)

<br>

### GET [`/v1/presets/:id`](/public/v1/presets)

* **Accept:** `'text/plain'`

<sup><strong>Suggested Request Headers</strong></sup>

```ts
// note: simplified; not actual type definition
export type BakedexApiPresetsResponse = BakedexApiBunsResponse & {
  ntt: 'presets';
  payload: CalcdexPokemonPreset[];
};
```

> <sup><strong>Source:</strong> <a href="https://github.com/doshidak/showdex/tree/master/src/interfaces/api/BakedexApiPresetsResponse.ts"><code>@showdex/interfaces/api/BakedexApiPresetsResponse</code></a></sup>  
> [`@showdex/interfaces/calc/CalcdexPokemonPreset`](https://github.com/doshidak/showdex/tree/master/src/interfaces/calc/CalcdexPokemonPreset.ts)

<br>

### GET [`/v1/supporters/:id`](/public/v1/supporters)

* **Accept:** `'text/plain'`

<sup><strong>Suggested Request Headers</strong></sup>

```ts
// note: simplified; not actual type definition
export type BakedexApiTiersResponse = BakedexApiBunsResponse & {
  ntt: 'tiers';
  payload: ShowdexSupporterTier[];
};
```

> <sup><strong>Source:</strong> <a href="https://github.com/doshidak/showdex/tree/master/src/interfaces/api/BakedexApiTiersResponse.ts"><code>@showdex/interfaces/api/BakedexApiTiersResponse</code></a></sup>  
> [`@showdex/interfaces/app/ShowdexSupporterTier`](https://github.com/doshidak/showdex/tree/master/src/interfaces/app/ShowdexSupporterTier.ts)

<br>
