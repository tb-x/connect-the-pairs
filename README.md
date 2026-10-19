# Connect the Pairs

A maker for printable picture puzzles for young kids (around age 5). Each puzzle is a grid of squares with pairs
of matching pictures in it (two suns, two fish, two cars…), and the child joins each pair with a line. The sheets
have no words, so they work for kids who can't read yet, and they're made for a black-and-white printer. The
answer is hidden in a QR code: scan it with a phone camera to watch coloured lines draw each pair's way.

Live at **https://tb-x.github.io/connect-the-pairs/**

## How it plays

- Print a sheet. Each grid has a few pairs of pictures, and a little example at the top left: two rings in a row
  of squares, joined by a line.
- Join each pair of matching pictures with a line through the squares, going up, down, left or right (never
  diagonally).
- Lines may not cross, and two lines may not go through the same square.
- For an extra challenge, fill every square: when every square has a line through it, there's only one way to
  join the pairs. A young child can ignore that and just join the pairs any way that works.
- Scan the QR code at the top right of the grid. The answer page draws each pair's line in its own colour, one
  after another, and the two pictures hop when their line is done. Tap the grid to draw it again.

## The maker page

- **Size**: 4 × 4 (3 or 4 pairs), 5 × 5 (4 or 5 pairs), 6 × 6 (5 or 6 pairs) or 7 × 7 (6 to 8 pairs).
- **Per page**: 1, 2 (one above the other) or 4 grids on each A4 page.
- **Pages**: how many pages to print at once (up to 20).
- **New puzzles** makes new puzzles for every page, ↻ on a page makes new ones for that page only, and **Print**
  prints them. The settings are remembered in this browser.
- Clicking a QR code on screen opens its answer page.
- The pictures are a sun, star, heart, fish, apple, flower, moon, house, car and tree, each pair a different one.
  Every picture is a black outline, so it can be coloured in.

How a puzzle is made: a random path that runs through every square once (a snake, shuffled with hundreds of
"backbite" moves), cut into lines of at least 3 squares. The cuts are placed so that no line runs alongside
itself, which keeps the lines simple, and the two ends of a line are never next to each other. The ends of each
line become a pair of pictures. A solver then checks that, with every square used, the puzzle has exactly one
answer; if not, it tries again. A puzzle takes a few milliseconds, so even 20 pages of 7 × 7 come up in well under
a second.

The QR code holds the whole puzzle in the link: the grid size, then five digits for each pair (its picture, then
its two squares, numbered row by row), e.g. `#5x5.10712505132…`. The digits go into the QR code in its numeric
mode, which packs them tightly, and every QR dot is at least 0.4 mm. The answer page solves the puzzle again from
the link. Nothing is stored anywhere, so a printed sheet keeps working for as long as this page is online. The QR
always points at the published page on GitHub Pages, even when the maker runs on a local server.

## Running it

It's a single `index.html` with no build step. Serve the folder and open it:

```bash
python -m http.server 5219
```

Then go to http://localhost:5219/. Opening `index.html` straight from disk works too. The QR codes are made
by [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) 1.4.4 (MIT), loaded from cdnjs.

After the first visit the page also works offline and can be added to a phone's Home Screen.

## Tuning knobs

Named constants at the top of the script in `index.html`:

| Constant | Default | What it does |
| --- | --- | --- |
| `DEFAULTS` | 5 × 5, 1 a page, 1 page | Starting settings |
| `PAIRS` | 4: 3–4, 5: 4–5, 6: 5–6, 7: 6–8 | The sizes in the Size list, and how many pairs each has |
| `MIN_LINE` | 3 | Fewest squares in one pair's line, counting both pictures |
| `BACKBITES` | 20 | Shuffling moves per square for the random path through every square |
| `TRIES` | 400 | Paths to try before giving up on a puzzle (one is nearly always found in the first few) |
| `SHEETS_MAX` | 20 | Most pages printed at once |
| `GRID` | 1, 2 (stacked), 4 (2 × 2) | How the grids sit on a page |
| `MAX_CELL` | 36 mm | Biggest square on the sheet |
| `PIC` | 0.78 | A picture's size, as a share of its square |
| `QR_SIZE` | 24 / 22 / 20 mm | Answer QR code with 1, 2 or 4 grids a page |
| `QR_MODULE` | 0.4 mm | Smallest QR dot: the QR code grows if it would be smaller |
| `LINE_W` | 0.3 | Answer line thickness, as a share of a square |
| `DRAW_SPEED` | 0.11 s | Time per square while the answer page draws a line |
| `COLOURS` | red, blue, green, orange, purple, pink, teal, brown | One line colour per pair on the answer page |
| `PUBLIC_URL` | `https://tb-x.github.io/connect-the-pairs/` | Where the QR codes point |
