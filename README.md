# Liza Beth — author site

The four-page author site for Liza Beth. Plain HTML, no build step, no dependencies.
Open `index.html` in a browser and it works — including straight off a USB stick.

| File | What's on it |
|---|---|
| `index.html` | Hero with the gilt lockup and arched window, three cloth-bound book nav cards, featured novel, signup — and the LB dragon |
| `about.html` | *About Liza* with her author portrait (`liza-beth-portrait.jpg`) in a gilt oval frame over the title band, her signature (`liza-beth-signature.png`), and the riddle card with its "turn off the lights" switch |
| `books.html` | *The Fall Before Flight* — cover art, back-of-book summary, **Liza's progress** bars, Chapter One |
| `letters.html` | *Letters from Liza* — "Leanie and Meanie" (from 2 Nov 2026) and "Moon Gold and Silver Horses" under **Short stories**, a Substack card, signup |

Supporting files: `favicon.ico`, `favicon-32.png`, `apple-touch-icon.png`, `icon-512.png`, and
`fall-before-flight-cover.jpg` (the rough-draft cover, used on the home and Books pages).

Everything else — the brand stylesheet, the LB dragon monogram, the arched-window
illustration, all the JavaScript — is inlined in each page. The only outside request is the
Google Fonts stylesheet for Spectral, Crimson Pro and Source Sans 3; if that ever fails to
load, the pages fall back to Georgia and a system sans and still read correctly.

## The Mailchimp signup form

The signup form appears on all four pages and **is wired to the live audience** — it posts
to `gmail.us8.list-manage.com` and real addresses land in Mailchimp. Under it sits a
`mailto:lizabethletters@gmail.com` link, kept as a fallback for anyone whose browser blocks
the form.

You only need what follows if you ever regenerate the form in Mailchimp.

Mailchimp moves this menu around; as of September 2026 it's **Forms → Other forms → Create
new form → Create embedded form**, then **Copy Code**. In the code it gives you, find the
`<form action="...">` line:

```
action="https://gmail.us8.list-manage.com/subscribe/post?u=XXXX&id=YYYY&f_id=ZZZZ"
```

Each of the four HTML files carries those values in **two** places, and they have to agree:

1. the `action` URL on the `<form>` tag, and
2. the hidden input just above the Subscribe button, named `b_<u>_<id>`.

That hidden input is Mailchimp's bot trap. If its name stops matching the `u` and `id` in
the action URL, Mailchimp silently rejects every signup — so never update one without the
other. Check the host at the front of the URL too; the `usNN` number is account-specific.

### Testing it

Subscribe with your own address and confirm it appears under **Audience → All contacts**. A
signup that seems to work but never arrives almost always means the bot-trap name and the
action URL have drifted apart.

## Two other things you'll want to change

### 1. Update the progress bars

In `books.html`, search for `lb-progress`. There are two blocks — First draft and Editing.
Each one carries the number in three places, and **all three have to match** or the bar and
the screen-reader label will disagree:

```html
<span class="lb-progress__pct">60%</span>          <!-- the number readers see -->
     aria-valuenow="60"                             <!-- the number screen readers hear -->
<span class="lb-progress__fill" style="width:60%">  <!-- the length of the bar -->
```

Add `lb-progress__fill--done` to the fill's class list when a stage hits 100% — that's what
turns the bar green instead of blue.

To add a stage, copy a whole `<div class="lb-progress">…</div>` block, paste it after the
last one, and change the name and the three numbers.

### 2. Add a letter

*Letters from Liza* opens on a page of cards, grouped by kind ("Short stories"). Clicking a
card opens that letter on its own, at `letters.html#its-id`, with a link back to all letters.
Each letter is two pieces in `letters.html`, and they have to share an id:

1. **The card**: an `<a class="lb-letter-card" href="#the-id">` inside the right
   `lb-letters__list`. Copy the existing one, then change the `href`, the title, the
   "5 min read" (about 220 words a minute), the short excerpt and the cover.
2. **The letter**: an `<article class="lb-letter" id="the-id">` inside
   `<section id="letters-read">`. Copy the existing one, then change the id, title, read
   time and cover, and put the story in `<div class="lb-prose lb-prose--story">`, one
   `<p>…</p>` per paragraph.

**Covers.** Each card and letter has a `<span class="lb-letter-cover">` / `<div class="lb-letter-cover">`.
To use a picture, put the image file next to the page and replace what is inside the cover
with `<img src="your-image.jpg" alt="what the picture shows">`. It is cropped to fit
automatically.

**Scheduling a letter.** Add `data-publish="YYYY-MM-DD"` to both the card and the
`<article>`, and they stay hidden until that date (in the reader's own time zone), then appear
on their own. "Leanie and Meanie" uses this for 2 November 2026. The "… letters so far" count
updates itself. (The text is in the page's source before then, so anyone viewing the source
could read it early.)

For a new kind of letter, copy a whole `<div class="lb-letters__group">` and rename its
heading. The "One letter so far" count is plain text, so update it by hand.

## Publishing it

The site is static, so anything that serves files will host it. For GitHub Pages: **Settings
→ Pages → Source: Deploy from a branch → main / (root)**. It goes live at
`https://clairewellenkamp.github.io/Liza_Beth_Website/` within a minute or two. A custom
domain is added on that same settings page.

## Content

All the prose is Liza's own, verbatim from her documents — *About Liza*, the back-of-book
summary, the Chapter One excerpt, and "Moon Gold and Silver Horses". Nothing was written on
her behalf; the only authored text is interface labels and two sentences on the signup card.

Two things are deliberately absent until Liza says otherwise: a series name (the book is
labelled "Book One") and publication dates on letters. The cover image is the rough draft;
to swap in the final art, replace `fall-before-flight-cover.jpg` (and update the `width` /
`height` on its two `<img>` tags if the proportions change).

## Design

Built to the Bag End · Blue Door brand kit v1.1 — ten colour tokens, three typefaces, a 4px
spacing scale. A few rules the stylesheet holds to, worth knowing before editing: no sixth
colour, no dark mode (the riddle card's "lights off" is a single dark card, not an inverted
page), gold is ornament only and never running text, and one gilt button per page.

## The home page extras

**The book cards** open when a mouse hovers over them (or a keyboard lands on them), so the
blurb inside shows before anyone clicks. On phones and tablets a tap opens the cover and then
follows the link, as before. Each cover is an inline SVG — gilt frame, beading, corner
fleurons, fuchsia vines and a medallion scene — over a cloth-textured board. The cloth colour
comes from one class on the `<a class="lb-book …">`: `lb-book--blue`, `lb-book--red`, or none
for ink. The gilt is the one place gold is used for lettering, because that is what a stamped
cover is.

**The LB dragon** is the winged dragon from the Claude Design handoff (*Dragon Reference
Sheet v2* and `DRAGON_ANIMATION_SPEC.md`). Its skeleton, colours and wingbeat are the
reference's own code, inlined at the bottom of `index.html` so the page still opens straight
from a folder. Its home is on top of the arched window in the hero, where it stands and
breathes. As you scroll down it takes off, seen from the side, nose-down, flying from side to
side through the margins and landing on each book, the *Fall Before Flight* cover and the
signup card. Scroll back up and it turns round and climbs, nose-up, to the perch above on
its own path instead of rewinding. Its speed and turning are capped, so a fast scroll makes
it glide to catch up; if the page leaps a long way, it flies in from just off-screen.
Clicking a link or button sends it on a quick swoop over that spot in the top view, as the
spec describes. Landing spots come from where those things actually are, so it follows the
layout on any screen size. It never blocks clicks, screen readers ignore it, and anyone with
"reduce motion" turned on sees it standing still on the arch.

**Page heads.** The title band at the top of About, Books and Letters from Liza is the cloth
colour of the book card that leads there: blue, red and ink. Each one is a single class on the
`lb-pagehead` div (`lb-pagehead--blue`, `--red` or `--ink`).
