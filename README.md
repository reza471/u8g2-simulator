# u8g2 Font Simulator

A single-file, fully offline HTML tool for previewing text, fonts, and simple
graphics on a simulated monochrome LCD (SSD1306-style), using **real u8g2
font bitmaps** — not approximations.

Built for testing [u8g2](https://github.com/olikraus/u8g2) fonts before
flashing them to real hardware (Arduino, ESP32, STM32, etc.).

![screen preview placeholder](docs/screenshot.png)

## Why

u8g2 fonts are stored as compressed bitmap data compiled into your firmware.
Picking the right font/size for a small screen usually means flashing,
looking, and re-flashing. This tool decodes the *actual* u8g2 font format in
the browser, so you can preview pixel-exact text and layout before touching
hardware.

## Features

- **All 2043 u8g2 fonts**, embedded and decoded pixel-exact (no external
  font files, no network calls — works fully offline once downloaded).
- **Real decoder**, ported line-by-line from u8g2's own `u8g2_font.c`, not a
  visual approximation.
- **Any screen size** — presets for 128x64, 128x32, 96x16, 72x40, 64x48,
  256x64, 240x240, or type your own width/height.
- **Searchable font list** — filter by name, click to select, or focus the
  list and use ↑ / ↓ to step through matches.
- **Text objects** — add multiple independent strings, each with its own
  position and font, to lay out a whole screen from several sentences.
  Click any text object in the list to rename it in place.
- **Drawing objects** — Line, Rect outline, Rect filled, Circle, and Single
  pixel, similar to u8g2's `drawLine` / `drawFrame` / `drawBox` /
  `drawCircle` / `drawPixel`. Reorder with ▲ ▼, remove with ×.
- **Manual pixel drawing** — click (or drag) directly on the simulated
  screen to toggle individual pixels, like a tiny bitmap editor.
- **Save / Load** — export the whole page state (screen size, text, font,
  objects, manual pixels) to a `.json` file and reload it later.
- **Live metrics** — line count, max text width, and a fits/doesn't-fit
  check against the current screen size.

## Usage

1. Download `u8g2_font_simulator_all.html` (or clone this repo).
2. Open the file in any modern browser. No server, no build step, no
   internet connection needed.
3. Pick a font from the list (or search by name).
4. Type text, add objects, or draw pixels directly.
5. Once you're happy with a font, copy its exact name (shown under
   **Font:**) into your firmware code:

   ```cpp
   u8g2.setFont(u8g2_font_6x10_tf);
   u8g2.drawStr(0, 10, "Hello u8g2!");
   ```

## How the font decoding works

u8g2 fonts use a custom run-length-encoded bitmap format (documented on the
[u8g2 wiki](https://github.com/olikraus/u8g2/wiki/u8g2fontformat)). This
project embeds the actual compressed byte arrays from u8g2's
`csrc/u8g2_fonts.c` and includes a JavaScript decoder ported from
`csrc/u8g2_font.c`, so every glyph you see here is exactly what would be
drawn on real hardware — not a substitute font or a rasterized approximation.

## Project structure

```
u8g2_font_simulator_all.html   # the whole app: markup, decoder, font data, UI logic
```

Everything lives in one file on purpose — download it once, and it keeps
working offline forever, with no dependency on this repo staying online.

## Credits

- Font data and font format: [olikraus/u8g2](https://github.com/olikraus/u8g2)
  (see its repository for font licenses — most are BSD/X11/OFL, check
  individual font headers in u8g2 for exact terms).
- This simulator is an independent tool and is not affiliated with the u8g2
  project.

## License

MIT for the simulator code in this repository. Embedded font data retains
its original licensing from the u8g2 project.
