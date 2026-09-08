# Battleship

Classic Battleship in a single self-contained `index.html` — no backend, no build step, no dependencies. Human player vs. a hunt/target AI opponent.

## Play locally

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Rules

- 10x10 grid per side, standard fleet: Carrier 5, Battleship 4, Cruiser 3, Submarine 3, Destroyer 2.
- Place your fleet manually (select a ship, `R` or **Rotate** to turn it, click to place; click a placed ship to pick it back up) or hit **Randomize**, then **Start game**.
- Turns alternate, one shot each. Already-fired cells are ignored, as are clicks during the AI's turn or after the game ends.
- Grey dot = miss, red X = hit, dark red = sunk ship. Clicking a cell you have already fired at pulses the cell and says so in the status line; it never costs a turn. First side to sink all five ships wins.
- **Open-information variant:** every hit names the ship and its hit count (e.g. "Cruiser 2/3"), symmetrically for both sides. A permanent notice above the boards explains this before the first shot.

## AI opponent

No external API or LLM. Two modes:

- **Hunt** — fires at a random untried cell restricted to a parity lattice (`(row + col) % stride === 0`), which halves the search space without ever skipping a ship. The stride is recomputed whenever a ship sinks, from the smallest surviving ship's size, and the AI falls back to any untried cell if the parity set is exhausted, so the size-2 Destroyer can always be found.
- **Target** — after a hit, the four orthogonal untried neighbours are queued. Once two hits line up, the queue is narrowed to the cells extending that line at either end. When the ship sinks, its leftover candidates are dropped and the AI returns to hunting.

## Deploying to GitHub Pages

1. Repo → **Settings** → **Pages**.
2. Under **Build and deployment**, set **Source** to *Deploy from a branch*.
3. Choose branch `main` and folder `/ (root)`, then **Save**.
4. Wait for the Pages deployment to finish (see the **Actions** tab or the banner on the Pages settings screen).
5. The site is served at `https://<user>.github.io/<repo>/` — for this repo, https://ivanporahi.github.io/cognition-battleship-IAG/

No workflow file or build configuration is needed: `index.html` at the repo root is published as-is. The site is fully static, so it also works from any file server or by opening the file directly.
