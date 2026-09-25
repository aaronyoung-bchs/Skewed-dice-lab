# The Skew Question

*Two dice, one very strange shape. Can a die like this really be fair? Roll, record, and decide.*

A single-page dice-rolling interactive for the **January 2027 Mu Alpha Theta monthly challenge** at Bullitt Central High School. One die is a standard cube. The other is modeled on The Dice Lab's **Skew d6**, a real slanted die whose faces are parallelograms.

**Live page:** <https://aaronyoung-bchs.github.io/Skewed-dice-lab/> *(available once GitHub Pages is enabled; see [Deploying](#deploying-to-github-pages))*

> **Status:** in development. See the [build checklist](#build-checklist).

---

## What the page does

The page runs a probability experiment. It rolls the dice, keeps your results, and lets you export them. All analysis happens outside the page.

- **Two dice:** a Standard Die and a Skew d6, each drawn in its own shape.
- **Rolling in batches:** roll 1, 5, or 10 at a time for each die.
- **Running results:** each die shows the count for every face and the total number of rolls, as text.
- **Roll limit:** each die allows up to 10,000 rolls in total. Saved rolls count toward the limit. Resetting a die clears its data and restores the full allowance.
- **Saved in your browser:** results stay through refreshes and return visits on the same device and browser.
- **CSV export for each die:** two files per die.
  - the full roll history in order (`roll_number, face`)
  - a summary of counts for each face

  Teammates on different devices can combine their data using these exports.
- **Reset:** each die resets on its own, after a confirmation step.

### Privacy

There are no logins, no tracking, and no data collection. Roll data never leaves your browser unless you export it. Clearing your browser's site data erases it.

---

## Credits

- **Skew d6:** designed and sold by [The Dice Lab](https://www.mathartfun.com/thedicelab.com/SkewDice.html). The drawing on this page is an original rendering. It does not use their photos, packaging, or logos.
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

## Reusing the page with different dice

*Filled in once `index.html` exists.* This section will explain where the dice configuration lives, how to change a die's name, shape, or face weights, and when to change the storage key so old saved data doesn't carry over.

---

## Build checklist

- [x] Settle open decisions (name, theme, credits)
- [x] Set up the repository foundation (README, license, Pages config)
- [ ] Build the page skeleton and intro panel
- [ ] Add the dice configuration and weighted rolling logic
- [ ] Draw the Standard Die and the Skew d6
- [ ] Add batch roll buttons (1 / 5 / 10) and a brief animation
- [ ] Add running counts and the 10,000-roll limit
- [ ] Save rolls in the browser (local storage)
- [ ] Add CSV export (full history and summary)
- [ ] Add reset with confirmation
- [ ] Check accessibility, Chromebook and phone layout, and performance at 10,000 rolls
- [ ] Enable GitHub Pages and tag `jan-2027`

---

## License

[CC0 1.0 Universal](LICENSE). You may copy, adapt, and reuse this page freely. The Skew d6 design and name belong to The Dice Lab.
