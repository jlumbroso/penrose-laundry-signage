# Penrose Laundry Signage

This repository creates signage diagrams (via [Penrose](https://penrose.cs.cmu.edu/)) for laundry bins.

## Files

- **`domain/laundyr-signs.domain`**: The Penrose _Domain_ file declares the `Category` type and our relevant predicates:
  - `HasLabel`
  - `HasFrequency`
  - `AcceptedItem`
  - `RejectedItem`
- **`style/laundyr-signs.style`**: The Penrose _Style_ file specifying how to draw signage for each `Category`.
- **`substance/`**: Five different _Substance_ files, one per laundry category:
  - `cottons.substance`
  - `synthetics.substance`
  - `underpants.substance`
  - `wool.substance`
  - `caps.substance`

## How it works

- Upon every push to `main` that changes a `.domain`, `.style`, or `.substance` file:
  1. GitHub Actions (see [`.github/workflows/compile-penrose.yml`](.github/workflows/compile-penrose.yml)) installs [roger](https://www.npmjs.com/package/@penrose/roger).
  2. For each Substance file, it produces a PDF sign in the `output/` directory.
  3. Then merges them all into one PDF, `laundyr-signs.pdf`.
  4. Publishes a GitHub Release with `laundyr-signs.pdf` attached.

Check the **Releases** tab for the generated PDF signage, or simply clone and run:

```bash
npm install -g @penrose/roger
mkdir -p output
# Example for cottons only:
roger trio \
  substance/cottons.substance \
  style/laundyr-signs.style \
  domain/laundyr-signs.domain \
  --out "output/cottons.svg"
```
