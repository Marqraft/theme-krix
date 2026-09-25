# Krix

The Kex documentation look for [Marqraft](https://github.com/Marqraft/cli):
the warm honey/cream palette of kex.run, Fraunces / Plus Jakarta Sans /
JetBrains Mono, a dark code surface with Kex syntax colouring, and the
sidebar, crumbs and reading-order footer of docs.kex.run.

## Use it

In a site's `marqraft.jsonc`, point `theme` at this directory:

```jsonc
{ "theme": "./themes/theme-krix" }         // vendored into the site, e.g. a git submodule
{ "theme": "/path/to/theme-krix" }         // a checkout elsewhere, for theme development
```

Or create a site with it:

```sh
marq new my-guide --theme /path/to/theme-krix
```

Marqraft copies the theme into the site's ignored `.marqraft/cache/` on load,
keyed by content, so edits to this directory show up on the next page load of
`marq dev` and in the next `marq build`.

## Collections

A site is a set of collections, such as **Guide** and **Tutorial**. Each
collection is a top-level page in the navigation (usually with the
`landing` template), and the pages nested under it are its chapters:

```text
Guide          /            header link, collection index
  Overview     /overview/   sidebar, reading order
  Syntax       /syntax/
Tutorial       /tutorial/
  First steps  /tutorial/first-steps/
```

The header lists the collections; the sidebar, breadcrumbs, version badge and
previous/next links follow the collection of the current page. The home page
(`/`) belongs to no collection: give it the `home` template, which drops the
sidebar and the chapter list. In `marq dev`,
**+** in the header creates a collection, and **New page** in the sidebar
adds a page to the current one. Collection order is changed from a
collection's menu in the header; page order by dragging in the sidebar.

## Reference pages

`reference.html.ket` frames generated API documentation — Tey's
`tey docs build --format fragments`, served by a Marqraft mount with
`"format": "fragments"` and `"template": "reference"`. It
adds an "On this page" rail from the page's `tocHtml` data. `style.css` carries docgen's reference styles (kind badges,
signatures, trait tags); keep them in step with
`tey/src/tey/docgen/css.kex` in kexhq/kex.

## Header links

After the version badge, the header carries ordinary links, starting with
**kex.run ↗**. In `marq dev`, hover a link to rename it, point it at a page
or a URL, move or remove it; **+** adds one. The site stores them in
`.marqraft/menus.json`. External links get an arrow.

## Favicon and uploads

The theme ships the Kex icon (`assets/icon.png`) as the favicon and puts
uploaded images in `public/assets/images/`. A site can override both in
`marqraft.jsonc`, or pick a favicon under **Theme → Site** in `marq dev`:

```jsonc
{ "favicon": "/assets/images/my-icon.png", "uploads": { "images": "media" } }
```

## Pages

| template (label) | use |
|-----------|-----|
| `page` (Page) | a page in a collection: sidebar, crumbs, prose, previous/next within the collection |
| `landing` (Collection) | a collection's index: sidebar, title, description as the lede, body, list of its pages |

Page frontmatter `description` becomes the `<meta name="description">` and the
index lede. Kex fences (```` ```kex ````) and Code blocks with language `kex` are
highlighted at build time; the published site carries no JavaScript.

## Settings

| name           | default                  | used for |
|----------------|--------------------------|----------|
| `brandSub`     | `docs`                   | the small label after the site title |
| `homeLabel`    | `kex.run`                | header and footer link text |
| `homeUrl`      | `https://kex.run/docs`   | header and footer link |
| `accent`       | `#8a4f0a`                | prose link colour (`--marq-accent`) |
| `contentWidth` | `48` (rem)               | reading column width (`--marq-contentWidth`) |
| `logo`         | none                     | image before the site title |

The site title (`title` in `marqraft.jsonc`) is the brand, e.g. `kex`.

## Blocks

- **Kex code** (`/code`): `<marqraft-code language="kex" filename="…" caption="…">`
- **Tabs** (`/tabs`): repeatable areas
- **Callouts** are GitHub alerts in plain Markdown (`/callout` inserts one), so the
  files read the same on GitHub:

  ```markdown
  > [!TIP]
  > Type `/` for blocks.
  ```

  Kinds: `NOTE`, `TIP`, `IMPORTANT`, `WARNING`, `CAUTION`.

## Search and versions

The version badge shows the collection and the version the page documents
(`context["version"]`: the page's or its collection's `version:`
frontmatter, or a reference page's mount version). Marqraft renders it
through `<marqraft-content source="versions">`, which becomes a switcher
between a reference's versions once there is more than one. Pages without a
version, such as the home page, have no badge.

The header's search (`/` or ⌘K, `assets/search.js`) reads one file, the
site's index (`"search"` in `marqraft.jsonc`, passed as
`context["searchIndex"]`). Give each reference mount `"search":
"search.json"` and Marqraft merges docgen's entries for its newest version
into that index.

## Files

```text
assets/icon.png          favicon, served as /_theme/icon.png
marqraft-theme.jsonc     manifest: templates, settings, favicons, uploads, menus, blocks, commands
style.css                docgen's stylesheet, then Marqraft-specific rules
page.html.ket            chapter layout
landing.html.ket         guide index layout
home.html.ket            home page layout: no collection sidebar
reference.html.ket       generated reference layout
assets/search.js         search
starters/*.md            initial body of new pages
```

## License

MIT. `style.css` is the Kex documentation stylesheet from Tey docgen
(`tey/src/tey/docgen/css.kex` in kexhq/kex, MIT); see NOTICE.
