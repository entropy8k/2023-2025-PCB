# pcb #general archive viewer

browser-based viewer for the 2023–2025 pcb `#general` chat log export.

## files

| file | purpose |
|---|---|
| `index.html` | the viewer |
| `log_yyyy-mm.txt` | one month of chat log, e.g. `log_2025-08.txt`. |
| `manifest.json` | list of available months |

## features

- **month navigation** — prev/next buttons and a dropdown.
- **cross-file search** — searches every month's file for a whole-word match and lists results with a snippet.

## is this safe to open?

this is a fully static site, it's just one html file with an inline `<script>`, and text
files. there is no backend, no cookies, no accounts, and nothing installed on your
device by visiting it.

the **only** two network requests the page ever makes are, verbatim in the source:

```js
fetch('manifest.json')
fetch(filename(key))   // e.g. fetch('log_2025-08.txt')
```

both are relative paths, meaning they only ever request files sitting in the same
folder as `index.html` on the same site, never an external server. there are no
`<script src="...">` tags pulling in third-party js, no analytics, no cdn calls,
no tracking pixels, no external links that auto-open, and no code that reads or
sends your ip, location, or device info anywhere.

you don't have to take that on faith. `index.html` is plain, unminified,
readable html/js. everything above can be verified by:

- opening the page's **view source** (or right-click → *view page source*).
- opening browser **devtools → network tab** while using the site, you'll see it only ever fetches `manifest.json` and `log_*.txt` files from the same domain.
- searching the source for `fetch`, `<script src`, `http://`, or `https://`, the only matches are the two `fetch()` calls above.

## notes

- whole-word search is case-insensitive and capped at 300 results per search to avoid an unusable wall of matches on common terms — narrow your search term if you hit the cap.
- a full "search all" run has to download every month's file the first time (once fetched, months are cached in memory for the rest of the session).
