# The Skew Question

*Two dice, one very strange shape. Can a die like this really be fair? Roll, record, and decide.*

A single-page dice-rolling interactive for the **January 2027 Mu Alpha Theta monthly challenge** at Bullitt Central High School. One die is a standard cube. The other is modeled on The Dice Lab's **Skew d6**, a real slanted die whose faces are parallelograms.

**Live page:** <https://aaronyoung-bchs.github.io/Skewed-dice-lab/> *(available once GitHub Pages is enabled; see [Deploying](#deploying-to-github-pages))*

> **Status:** in development. See the [build checklist](#build-checklist).

---

## What the page does

The page runs a probability experiment. It rolls the dice, keeps your results, and lets you export them. All analysis happens outside the page.

- **Two dice:** a Standard Die and a Skew d6, each drawn in its own shape.
- **Rolling in batches:** roll 1, 5, or 10 at a time for each die. Each batch plays a short tumble and lists the faces just rolled.
- **Running results:** each die shows the count for every face and the total number of rolls, as text.
- **Roll limit:** each die allows up to 10,000 rolls in total. Saved rolls count toward the limit. If a batch would pass the limit, only the rolls that fit are made. At the limit, the roll buttons turn off and a short message suggests exporting. Resetting a die clears its data and restores the full allowance.
- **Saved in your browser:** results stay through refreshes and return visits on the same device and browser.
- **CSV export for each die:** two files per die, named with the die and the date (for example `skew-d6-roll-history-2027-01-15.csv`).
  - **Roll history:** every roll in order, with columns `roll_number,face`
  - **Summary:** columns `face,count`, one row per face plus a `total` row

  Teammates on different devices can combine their data using these exports.
- **Reset:** each die resets on its own. A confirmation box shows how many rolls will be erased, and **Cancel** has focus by default.

### Privacy

There are no logins, no tracking, and no data collection. Roll data never leaves your browser unless you export it. Clearing your browser's site data erases it.

---

## Credits

- **Skew d6:** designed and sold by [The Dice Lab](https://www.mathartfun.com/thedicelab.com/SkewDice.html). The dice drawings on the page were generated with AI assistance (Claude), as SVG shapes drawn by the page's own code. They do not use The Dice Lab's photos, packaging, or logos.
- **Classroom data:** the Skew d6 in this simulation is based on real rolls collected by AP Statistics teacher **Doug Tyson** and his students.
- **Challenge and page:** Aaron Young, Bullitt Central High School Mu Alpha Theta.

---

## Repository layout

```
index.html   The whole interactive: HTML, CSS, and JavaScript in one file
.nojekyll    Tells GitHub Pages to serve files as-is (no Jekyll processing)
README.md    This file
LICENSE      CC0 1.0 (public domain dedication)
```

The site has no build step, no server, and no required external libraries.

---

## Deploying to GitHub Pages

1. On GitHub, open the repository and go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
3. Choose branch **`main`** and folder **`/ (root)`**, then **Save**.
4. After a minute or two, the page is live at `https://aaronyoung-bchs.github.io/Skewed-dice-lab/`.

Pages serves whatever is on `main`, so anything merged to `main` goes live for students.

---

## Maintenance workflow

**Branches and pull requests.** Make changes on a separate branch and open a pull request into `main`. Test the page from the branch before merging, because merging publishes it.

**Version tags.** When a version goes out to students, tag it so that exact page can be restored or reused later:

```bash
git tag -a jan-2027 -m "Version used for the January 2027 challenge"
git push origin jan-2027
```

**Avoid changes during a live challenge.** Teams' saved rolls live in their browsers. Changing the storage format or the dice while a challenge is running could erase or mix up their data. Make changes between challenges, or bump the storage key on purpose (see below).

**Teacher-only materials** (answer key, scoring checklist, debrief notes) are kept **outside this repository**, because the repository is public.

---

## Look and theme

The page uses the Bullitt Central High School colors, defined once as CSS variables at the top of `index.html`:

| Token | Color | Used for |
| --- | --- | --- |
| `--bchs-maroon` | `#4B2D2F` | header, headings, buttons, links |
| `--bchs-gray` | `#B0B1AC` | borders, header stripe, table lines |

Contrast was checked against WCAG: maroon on white is 12.2:1 and maroon on gray is 5.7:1. Gray on white is only 2.2:1, so **gray is never used for text**.

The layout shows two columns on wide screens and one column on phones and narrow Chromebook windows. All buttons are at least 48 px tall for touch.

---

## How the page works

All the code is in `index.html` and is commented section by section:

| Section in `index.html` | What it does |
| --- | --- |
| `DICE CONFIGURATION` | the list of dice: names, shapes, colors, encoded weights |
| `WEIGHT ENCODING` | turns weights into the stored string and back |
| `ROLLING LOGIC` | weighted random rolls |
| `SAVED DATA` | saving and loading each die's history in the browser |
| `DRAWING` | draws each die as an SVG and shows the rolled face on top |
| `PAGE LOGIC` | buttons, counts, the roll limit, CSV export, reset |

**Randomness.** Rolls use the browser's secure random generator (`crypto.getRandomValues`). Each face's probability is its whole-number weight divided by the total of all six weights. Rejection sampling keeps every outcome exactly as likely as its weight says. Every roll is independent: the page never adjusts the rolls to match earlier results.

**Saved data.** Each die's history is stored in the browser's `localStorage` as one string of digits, one character per roll (about 10 KB at 10,000 rolls), under the key `skewQuestion.v1.<die id>`. Counts are rebuilt from the history when the page loads. The screen updates once per batch, not once per roll. If the page is open in two tabs, they stay in sync.

**Drawings.** Each die is a cube drawn in an oblique view that shows three faces. The Skew d6 applies a shear to the cube's corners, so every face becomes a parallelogram, and its pips slant with the faces. Visible faces follow real die layout: opposite faces add to 7.

---

## Reusing the page with different dice

Everything about the dice is in the `DICE` list near the top of the script in `index.html`. Each entry looks like this:

```js
{
  id: 'standard',            // storage key and element ids (letters only, keep it unique)
  name: 'Standard Die',      // shown on the page
  file: 'standard-die',      // start of the exported file names
  shape: 'cube',             // 'cube' or 'skew'
  colors: { top: '#FFFFFF', front: '#ECECE9', side: '#D3D3CF', edge: '#4B2D2F', pip: '#1F1A1B' },
  weights: 'MS4xLjEuMS4xLjE=',   // encoded; this one is [1, 1, 1, 1, 1, 1]
},
```

**To change a die's weights:**

1. Choose six whole numbers, one per face in order 1–6. Each face's probability is its number divided by the total. `[1, 1, 1, 1, 1, 1]` is a standard die, and `[2, 1, 1, 1, 1, 1]` makes face 1 twice as likely as each other face.
2. Open the page in Chrome and open the console (**Ctrl+Shift+J**, or **Cmd+Option+J** on a Mac).
3. Run `encodeWeights([2, 1, 1, 1, 1, 1])` and copy the string it prints.
4. Paste that string into the die's `weights` value.

The weights are encoded only so they aren't in plain view. The encoding is not secret: anyone reading the code can decode it.

**To change how slanted the Skew d6 looks,** edit the `SHEAR` values in the `DRAWING` section. `0` means no slant.

**Start every browser fresh after changing the dice.** Rolls saved under the old dice would otherwise mix with the new ones. Change `STORAGE_PREFIX` (for example from `skewQuestion.v1.` to `skewQuestion.v2.`). Old data is then ignored.

**Other settings** near the top of the script are `BATCH_SIZES` (currently 1, 5, 10) and `MAX_ROLLS` (currently 10,000).

---

## Build checklist

- [x] Settle open decisions (name, theme, credits)
- [x] Set up the repository foundation (README, license, Pages config)
- [x] Build the page skeleton and intro panel (BCHS colors, responsive layout)
- [x] Add the dice configuration and weighted rolling logic (encoded weights, secure randomness)
- [x] Draw the Standard Die and the Skew d6
- [x] Add batch roll buttons (1 / 5 / 10) and a brief tumble animation
- [x] Add running counts and the 10,000-roll limit
- [x] Save rolls in the browser (local storage)
- [x] Add CSV export (full history and summary)
- [x] Add reset with confirmation
- [ ] Final review: test on a real Chromebook and phone, check accessibility with a keyboard and a screen reader
- [ ] Enable GitHub Pages and tag `jan-2027`

---

## License

[CC0 1.0 Universal](LICENSE). You may copy, adapt, and reuse this page freely. The Skew d6 design and name belong to The Dice Lab.
