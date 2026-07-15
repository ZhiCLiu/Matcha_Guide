# Steeped

**Matcha, read from the source.** Ten brands and eight tools, ranked from what people
actually say on [r/matcha](https://www.reddit.com/r/matcha/) and
[r/MatchaEverything](https://www.reddit.com/r/MatchaEverything/) — not from whoever
pays the best affiliate rate.

### → [Read the guide](https://github.com/ZhiCLiu/Matcha_Guide.git)

Every "best matcha" list is someone's affiliate income. This one isn't. No ads, no
sponsored placement, no brand moves up. Each verdict links back to the thread it
came from, so you can check my work.

**Where it's soft:** the sentiment scores are my read of recurring themes, not a
computation. One tool in here has no independent community feedback at all, and its
row says so. Next version computes the numbers from the Reddit API and cites specific
threads.

If it's useful, a ⭐ tells me it's worth updating next month.

---

<details>
<summary><b>Running it yourself</b></summary>

Two files, no build step, no backend.

- `index.html` — the whole site
- `data.json` — the rankings; the only file that changes

```bash
python3 -m http.server 8000   # then open localhost:8000
```

(Opening `index.html` directly won't work — it fetches `data.json`, which `file://` blocks.)

**Before deploying,** set two variables at the top of the `<script>` in `index.html`:

```js
var GITHUB_REPO = "YOUR_USERNAME/matcha-guide";
var BMC_USER    = "YOUR_USERNAME";
```

Then push and turn on Pages: Settings → Pages → Deploy from a branch → `main` / root.

</details>

<details>
<summary><b>Editing the rankings</b></summary>

Everything lives in `data.json`. Edit, commit, push — Pages redeploys itself.

```json
{
  "rank": 1,
  "name": "Brand name",
  "tags": ["ceremonial", "latte", "budget"],
  "sentiment": 94,
  "mentions": 412,
  "trend": "up",
  "trend_note": "climbing",
  "pros": [{ "text": "What people like.", "source": "r/matcha", "detail": "context" }],
  "cons": [{ "text": "What they don't.", "source": "r/MatchaEverything", "detail": "context" }]
}
```

- `trend` — `up` / `down` / `steady`
- `source` — `r/matcha` / `r/MatchaEverything` / `vendor site` — decides where the citation links
- `tags` — drive the filters
- `gear[]` items also take `kind`, `verdict` (`buy` / `maybe` / `depends` / `skip`), `price`, `summary`

</details>

<details>
<summary><b>Design notes</b></summary>

Direction is **shade-grown**: matcha spends weeks under tarps before harvest, which is
where the color comes from. The page is that shaded interior — pine ink on warm stone,
with green reserved *only* for verdicts. Green is never decorative. Dissent is clay,
contested is gold.

Newsreader (editorial serif) against Inter Tight (data). Opinion journalism with numbers
under it.

The sentiment meter isn't a bar — it's **sifted powder**. Grain size and density both
encode the score: 95 is fine and dense, 66 is coarse and sparse, tiling an SVG sieve mask.
The same grain settles over "shade" in the headline on load. Don't swap it for a progress bar.

</details>

---

MIT. The rankings are opinion; the tea is not included.
