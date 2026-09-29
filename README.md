# Liza Beth — author site

The four-page author site for Liza Beth. Plain HTML, no build step, no dependencies.
Open `index.html` in a browser and it works — including straight off a USB stick.

| File | What's on it |
|---|---|
| `index.html` | Hero with the gilt lockup and arched window, three book-shaped nav cards, featured novel, signup |
| `about.html` | *About Liza*, the LB mark, and the riddle card with its "turn off the lights" switch |
| `books.html` | *The Fall Before Flight* — cover, back-of-book summary, **Liza's progress** bars, Chapter One |
| `letters.html` | *Letters from Liza* — "Moon Gold and Silver Horses" under **Short stories**, signup |

Supporting files: `favicon.ico`, `favicon-32.png`, `apple-touch-icon.png`, `icon-512.png`.

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

In `letters.html`, each letter is one `<article class="lb-card">`. Copy the existing one,
paste it below, and replace the title in the `<h3>` and the paragraphs inside
`<div class="lb-prose lb-prose--story">`. Each paragraph needs its own `<p>…</p>`.

The `lb-card__eyebrow` is the category label ("Short stories"). The count above the list —
"One letter so far" — is plain text, so update it by hand.

## Publishing it

The site is static, so anything that serves files will host it. For GitHub Pages: **Settings
→ Pages → Source: Deploy from a branch → main / (root)**. It goes live at
`https://clairewellenkamp.github.io/Liza_Beth_Website/` within a minute or two. A custom
domain is added on that same settings page.

## Content

All the prose is Liza's own, verbatim from her documents — *About Liza*, the back-of-book
summary, the Chapter One excerpt, and "Moon Gold and Silver Horses". Nothing was written on
her behalf; the only authored text is interface labels and two sentences on the signup card.

Three things are deliberately absent until Liza says otherwise: a series name (the book is
labelled "Book One"), publication dates on letters, and cover art (the cover is a
typographic placeholder on its neutral mat).

## Design

Built to the Bag End · Blue Door brand kit v1.1 — ten colour tokens, three typefaces, a 4px
spacing scale. A few rules the stylesheet holds to, worth knowing before editing: no sixth
colour, no dark mode (the riddle card's "lights off" is a single dark card, not an inverted
page), gold is ornament only and never running text, and one gilt button per page.
