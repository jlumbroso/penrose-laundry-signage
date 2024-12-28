# Penrose Laundry Signage

This repository creates signage diagrams (via [Penrose](https://penrose.cs.cmu.edu/)) for laundry bins.

## Files

- **`domain/laundry-signs.domain`**: The Penrose _Domain_ file declares the `Category` type and our relevant predicates:
  - `HasLabel`
  - `HasFrequency`
  - `AcceptedItem`
  - `RejectedItem`
- **`style/laundry-signs.style`**: The Penrose _Style_ file specifying how to draw signage for each `Category`.
- **`substance/`**: Five different _Substance_ files, one per laundry category:
  - `cottons.substance`
  - `synthetics.substance`
  - `underpants.substance`
  - `wool.substance`
  - `caps.substance`

## How It Works

- Upon every push to `main` that changes a `.domain`, `.style`, or `.substance` file:
  1. GitHub Actions (see [`.github/workflows/compile-penrose.yml`](.github/workflows/compile-penrose.yml)) installs [roger](https://www.npmjs.com/package/@penrose/roger).
  2. For each Substance file, it produces a PDF sign in the `output/` directory.
  3. Then merges them all into one PDF, `laundry-signs.pdf`.
  4. Publishes a GitHub Release with `laundry-signs.pdf` attached.

Check the **Releases** tab for the generated PDF signage, or simply clone and run:

```bash
npm install -g @penrose/roger@4.0.0-alpha.5
mkdir -p output
# Example for cottons only:
npx roger trio \
  substance/cottons.substance \
  style/laundry-signs.style \
  domain/laundry-signs.domain \
  --out "output/cottons.svg"
```

### Penrose Syntax

This project uses [Penrose](https://penrose.cs.cmu.edu/) to automatically layout signs for various laundry categories. **Penrose** is a system that takes three files—**Domain**, **Substance**, and **Style**—and renders a diagram by optimizing shape placements. Here’s what each file does:

1. **Domain (\*.domain):**
   Declares the _types_ and _relationships_ for a given domain. In our case, we have:
   - **`Category`**: a type of laundry category (e.g., COTTONS).
   - Predicates like **`HasLabel`**, **`HasFrequency`**, **`AcceptedItem`**, and **`RejectedItem`**.

2. **Substance (\*.substance):**
   Specifies _which_ objects and relationships to draw. For each laundry bin sign, we create:
   - A **`Category`** object (e.g., `C` for COTTONS).
   - Calls to predicates like `HasLabel(C, "COTTONS")`.

3. **Style (\*.style):**
   Tells Penrose _how_ to display the objects declared in Substance. Here, we define:
   - A full **`canvas { width=500, height=200 }`** block.
   - A **big green rectangle** for each `Category`.
   - Additional **text shapes** placed relative to that rectangle’s center (e.g., label text, frequency text, accepted/rejected items).

Penrose then **optimizes** the positions and sizes of these shapes, subject to your constraints (e.g., “the label text is centered on the rectangle”). When you run our GitHub Action or local build, **Roger** (the Penrose CLI) compiles the trio:

1. Reads the `.domain` and `.substance` (defining the logical objects).
2. Reads the `.style` instructions.
3. Outputs an **SVG** (ultimately converted into a PDF) with the shapes arranged automatically.

### Tweaks to the Style

You can easily customize the final diagram by editing the `.style` file. Some common tweaks include:

- **Change the background color** of the main sign:
  
  ```styl
  fillColor : #FFD700  -- example: turn the rectangle gold
  ```

- **Adjust the rectangle corners**:
  
  ```styl
  cornerRadius : 50    -- make extremely rounded corners
  ```

- **Reposition or resize**:
  
  ```styl
  center : (400, 300)  -- you could shift the sign to another part of the canvas
  width  : 600         -- or make it wider
  ```

- **Change fonts**:
  
  ```styl
  fontFamily : "serif"
  fontSize   : "36px"
  ```

- **Add more text**:
  For example, you could add a smaller “Notes” line underneath `freqText`, or define separate lines for specific items, etc.

By editing **just the Style**, you can change the entire look without altering the Domain or Substance structure. Penrose automatically re-optimizes everything to maintain a good layout.
