# Steeped — matcha guide (static v1)

A single-page matcha ranking site. Reads from `data.json`, renders a Top 10 with
community-sourced pros/cons, and links each claim back to r/matcha or
r/MatchaEverything. No backend, no build step.

## Files
- `index.html` — the whole site (HTML + CSS + JS inline)
- `data.json` — the only file you edit each month (holds both `products` and `gear`)

## Data sources
The guide draws on the two core matcha subreddits:
- **r/matcha** — larger, enthusiast-leaning; brand comparisons and ceremonial-grade depth
- **r/MatchaEverything** — newer, broader; lattes, budget picks, everyday/lifestyle use

Each pro/con carries a `source` field. Citation links auto-route to that
subreddit's search (`?restrict_sr=1`), so a claim tagged `r/MatchaEverything`
links into that sub, not r/matcha.

## Run locally
Opening index.html directly with file:// is blocked (it fetches data.json). Run:
    cd matcha-guide
    python3 -m http.server 8000
    # open http://localhost:8000

## Deploy to GitHub Pages
1. Create a new repo (e.g. matcha-guide).
2. Push:
    git init
    git add .
    git commit -m "v1: static matcha ranking"
    git branch -M main
    git remote add origin https://github.com/YOUR_USERNAME/matcha-guide.git
    git push -u origin main
3. Settings -> Pages -> Source -> main / root -> Save.
4. Live at https://YOUR_USERNAME.github.io/matcha-guide/

## Update monthly
Edit data.json, commit, push. Each product:
    {
      "rank": 1, "name": "Brand", "tags": ["ceremonial","latte","budget"],
      "sentiment": 94, "mentions": 412, "trend": "up", "trend_note": "climbing",
      "pros": [{ "text": "...", "source": "r/matcha", "detail": "..." }],
      "cons": [{ "text": "...", "source": "r/MatchaEverything", "detail": "..." }]
    }
- trend: "up" | "down" | "steady"
- source: "r/matcha" | "r/MatchaEverything" (controls which sub the link points to)
- tags drive the filter buttons: ceremonial, latte, budget

## Before launch — set two variables
Open `index.html`, find the config block at the top of the <script> tag:

    var GITHUB_REPO = "YOUR_USERNAME/matcha-guide";
    var BMC_USER    = "YOUR_USERNAME";

Set both. That wires the Star button, the live star count, and Buy Me a Coffee.
The star count calls `api.github.com` (public, no key). If the repo is private,
the call is rate-limited, or the user is offline, the count element removes itself
rather than showing a fake number.

Still a stub: "Suggest a brand or tool" -> point it at a Google Form or mailto.

## Why star ranks above coffee
The support section is a ladder, not two side-by-side buttons. Stars convert far
better than donations (free, one click, useful to the clicker) and — more to the
point — the star count is your demand signal. After you post to Reddit, the star
curve tells you whether anyone actually wants this. Donations won't. Keep the order.

## Posting to Reddit — post to each sub differently
Both subs have self-promotion rules; a bare link usually gets removed. Post the
content, put the link in the body or first comment, and read each sub's rules first.
- r/matcha: lead with the brand-comparison / source-tracing angle (enthusiast crowd).
- r/MatchaEverything: lead with "quick way to find a good everyday/latte matcha
  without overpaying" (broader, lifestyle crowd).

## Next (only after traffic validates demand)
- Replace hand-tuned sentiment with scores from the official Reddit API.
- Add a scheduled job + database.
- Add transparent affiliate links (labeled, never affecting rank order).

## Gear & tools section
`data.json` has a second array, `gear`, rendered as its own Notion database below
the brand rankings. Each item:

    {
      "name": "The Song Cha tea sifter weights",
      "kind": "Nice to have",        // Essential | Nice to have | Divisive | Underrated | Skip
      "verdict": "maybe",            // buy | maybe | depends | skip  -> drives the colored tag + filter
      "price": "~$12",
      "summary": "One-line what-it-is.",
      "pros": [{ "text": "...", "source": "r/matcha", "detail": "..." }],
      "cons": [{ "text": "...", "source": "r/MatchaEverything", "detail": "..." }]
    }

`source` can also be `"vendor site"`, which links to the product page instead of
a subreddit search. Used for The Song Cha sifter weights, since its only public
feedback right now is on the vendor's own site — flagged as thin data in the cons.

## Design notes (so future-you doesn't undo it)
Direction: "shade-grown." Matcha is grown under tarps for weeks — that's where the
color comes from. The page is that shaded interior: deep pine ink (`--tarp`) on warm
stone (`--paper`), with green (`--leaf`) reserved *only* for verdicts. Green is never
decorative. Dissent is clay, contested is gold.

Type: Newsreader (editorial serif, display) against Inter Tight (UI/data). The tension
is the point — opinion journalism backed by numbers.

Signature: the sentiment meter is not a bar. It's **sifted powder** — grain size and
density both encode the score (95 = fine + dense, 66 = coarse + sparse), tiling an SVG
sieve mask set in `--grain`. `meter()` in index.html computes both from the score.
The same grain settles over "shade" in the h1 on load. Don't swap it for a progress bar.