# Color Palette Generator

A full-screen color palette generator with multiple harmony modes, lockable swatches, and instant clipboard export. Built with pure HTML, CSS, and JavaScript.

![Color Palette Generator](https://img.shields.io/badge/HTML-CSS-JS-purple?style=flat-square) ![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)

---

## Features

- **7 harmony modes** — Random, Analogous, Complementary, Triadic, Pastel, Deep Dark, Neon
- **Lock individual colors** — keep favorites while regenerating the rest
- **Click any swatch to copy** its HEX code
- **Export all colors** — copies all 5 HEX codes to clipboard at once
- **Keyboard shortcut** — press `Space` to generate a new palette
- **Hover to expand** — swatches grow on hover, also reveals RGB value

---

## How to Use

1. Open `index.html` in any browser — or visit the live GitHub Pages link
2. Press **Space** or click **Generate** to create a new palette
3. Click a **swatch** to copy its HEX color code
4. Click the **lock icon** on a swatch to keep that color when regenerating
5. Use the **mode dropdown** to switch harmony styles
6. Click **Export** to copy all 5 hex codes at once

---

## How to Customize

All customization lives inside `index.html`. No build tools needed.

### Change the number of swatches

The palette is driven by an array of 5 colors. To change the count, update three places:

**1. The locked array (in the `<script>` block):**
```js
let locked = [false, false, false, false, false]; // one entry per swatch
```

**2. Each mode's loop count** (e.g. for Random mode):
```js
for (let i = 0; i < 5; i++) { // change 5 to your desired count
```

**3. The Complementary/Triadic arrays** — adjust the hardcoded hue arrays to match your count.

### Add a new harmony mode

In the `buildPalette` function, add a new `else if` block:

```js
} else if (mode === 'earth') {
  const earthHues = [25, 35, 45, 15, 55]; // warm brown/orange hues
  earthHues.forEach(h => next.push(hslToHex(h, rnd(30, 55), rnd(25, 45))));
}
```

Then add the option to the dropdown in the HTML:

```html
<option value="earth">Earth Tones</option>
```

### Change what "Export" copies

Find the export button click handler:

```js
document.getElementById('exportBtn').addEventListener('click', () => {
  const text = colors.map(c => c.toUpperCase()).join('\n'); // one hex per line
  navigator.clipboard.writeText(text).then(() => toast('All colors copied!'));
});
```

Change `'\n'` to `', '` for comma-separated, or build a full CSS block:

```js
const text = `:root {\n${colors.map((c,i) => `  --color-${i+1}: ${c};`).join('\n')}\n}`;
```

### Change the topbar colors

Find the `.topbar` CSS rule and update the background:

```css
.topbar {
  background: #0f0f1a; /* change this */
  border-bottom: 1px solid rgba(255,255,255,0.06);
}
```

### Adjust color saturation and lightness ranges

Each mode uses `rnd(min, max)` for saturation (S) and lightness (L). For example:

```js
} else if (mode === 'pastel') {
  for (let i = 0; i < 5; i++) {
    next.push(hslToHex((base + i * 65) % 360, rnd(55, 72), rnd(72, 84)));
    //                                                S: 55–72%  L: 72–84%
  }
}
```

Increase the L range for lighter colors, decrease for darker. Increase S for more vivid, decrease for muted.

---

## Tech Stack

| Layer | Tech |
|-------|------|
| Structure | HTML5 |
| Styling | CSS3 (flexbox, transitions, backdrop-filter) |
| Logic | Vanilla JavaScript (HSL color math) |
| Font | Inter (Google Fonts) |

---

## License

MIT — free to use, modify, and distribute.
