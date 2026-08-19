# Cyberpunk Theme — Sublime Merge

A Sublime Merge theme matching my Konsole profile and Neovim colorscheme, so a
diff here reads like the same file in the editor and the window doesn't clash
with the terminal next to it.

Derived from [Meetio Theme for Sublime Merge](https://github.com/meetio-theme/merge-meetio-theme)
(Palenight variant, MIT) with the palette replaced and some scope rules
re-pointed. Upstream is unmodified and still installable alongside this.

## Install

```sh
git clone <this-repo> ~/.config/sublime-merge/Packages/"Cyberpunk Signal"
```

Then in `Packages/User/Preferences.sublime-settings`:

```json
{
    "theme": "Merge Cyberpunk Signal.sublime-theme",
    "color_scheme": "Cyberpunk Signal.sublime-color-scheme"
}
```

Sublime Merge reloads both on save — no restart.

Paths on other platforms: `~/Library/Application Support/Sublime Merge/Packages`
(macOS), `%APPDATA%\Sublime Merge\Packages` (Windows).

## Where the colors come from

Two sources, which turned out to hold the same palette:

- `~/.local/share/konsole/cyberpunk-signal.colorscheme` (used by the
  `Nvim Coding` profile)
- the `base16-cerulean-signal-dark` overrides in
  `~/.config/nvim/lua/plugins/colorscheme.lua`

| variable | value | source |
| --- | --- | --- |
| `background` | `#000018` | Konsole `Background` / `base00` |
| `foreground` | `#ABB5C7` | `Foreground` / `base05` |
| `comments` | `#919CB2` | `Color0Intense` / `base03`, lightened — see below |
| `blue` | `#81A0FF` | `Color4Intense` / `base0D` |
| `cyan` | `#3FE0D8` | `Color6Intense` |
| `accent` | `#27BAB5` | `Color6` / `base0C` |
| `green` | `#2DBE70` | `Color2` / `base0B` |
| `purple` | `#B192F9` | `Color5` / `base08` |
| `pink` | `#F182E8` | `Color5Intense` / `base0E` |
| `red` | `#FF6B8F` | `Color1Intense` |
| `orange` | `#4FB1E2` | `Color4` / `base09` — see below |
| `yellow` | `#D394B9` | `Color3` / `base0A` — see below |
| `brown` | `#949ABA` | `ForegroundFaint` / `base0F`, lightened — see below |
| `violet` | `#A890DF` | nvim `SnacksIndentScope`, lightened — see below |
| `white` | `#E2E8F2` | `ForegroundIntense` |
| `cursor` | `#3DFF9A` | `Color2Intense` — chosen, see below |
| `diffAdded` | `#00FF95` | `Color2Intense`, max chroma — see below |
| `diffDeleted` | `#DB0677` | chosen, see below |
| `diffModified` | `#4FB1E2` | `Color4` |

### Choices that aren't a straight copy

**No orange, no yellow.** The nvim config deliberately drops both (`base09`
"was orange" → blue, `base0A` "was yellow" → mauve), so the `orange` and
`yellow` variables hold a blue and a mauve. Upstream used them for tag
annotations and the "unmerged" icon; those are mauve here, not yellow.

**Cursor.** Neither config sets a caret color, so `#3DFF9A` is a pick —
bright enough to locate, not used for anything else.

**The two diff colors are tuned, not copied.** `diffDeleted` sits in a gap:
the palette jumps straight from magenta (`#F182E8`, hue 305°) to red
(`#FF3860`, hue 348°) with nothing between, so `#DB0677` is hue 328° at 44%
lightness. `diffAdded` is `Color2Intense` (`#3DFF9A`) pushed to full chroma —
that colour already pins its green channel at 255, so the only way to make the
block read more neon is to drop lightness to 50% (chroma 0.76 → 1.00) and let
the alpha do the brightening.

The two sides run different alphas because their colours differ in luminance.
A darker base needs more overlay to register at all: `#DB0677` at the added
side's 0.16 would sit at 1.10:1 against the page, near upstream's invisible
1.08:1. The alphas are picked per side to land both blocks in the same
1.5–2.0:1 band.

**Accent ladder widened.** Upstream derives its panel shades as
`background +2/+4/+10%` lightness, tuned for Palenight's `#292D3E` (L≈20%).
Against `#000018` (L≈4.7%) those steps were invisible, so they're `+3/+6/+12%`
here. `accentLighter` now lands on Konsole's own `BackgroundIntense`
(`#0A1230`).

**Two hardcoded colors removed.** `highlightedButtonLightBg` and
`highlightedButtonDarkBg` were literal olive-green `hsl()` values, the only
palette colors upstream didn't route through a variable. They come from
`green` now.

## Diff blocks

One color per side, both at full chroma. Alphas are set so the block is
visible against `#000018` while body text on top of it stays above the 4.5:1
WCAG AA threshold — the `.char` (changed-word) variant is stronger than its
line so edited words read out of the changed line.

| | color | line | char | text on char |
| --- | --- | --- | --- | --- |
| added | `#00FF95` | `0.16` → `#00292C` | `0.23` → `#003B35` | 6.1:1 |
| removed | `#DB0677` | `0.34` → `#4A0238` | `0.46` → `#650344` | 6.2:1 |

Upstream had all four at `0.10`, which blends to ~1.10:1 against this
background — effectively invisible, and the `.char` highlight was
indistinguishable from its line.

`diffAdded` / `diffDeleted` stay bright because they also drive the 2px gutter
markers, which need to read as thin lines on near-black. To tune the blocks,
change the alphas in `schemes/Cyberpunk Signal.sublime-color-scheme`; to
change the depth of the color, darken the base.

`diffModified` is blue rather than upstream's yellow-then-mauve, so it stays
distinct from the pink removed marker in the gutter.

## Syntax scopes

Twelve scope rules were re-pointed where upstream's token → color assignment
disagreed with what base16 gives the same token in nvim:

| scope | was | now | base16 slot |
| --- | --- | --- | --- |
| `keyword.control` (+ conditional, import) | cyan | pink | `base0E` |
| `punctuation.definition.keyword` | cyan | pink | `base0E` |
| `keyword.other` | orange | pink | `base0E` |
| `keyword.operator.word` | cyan | pink | `base0E` |
| `storage.type` | purple | pink | `base0E` |
| `storage.modifier` | purple | pink | `base0E` |
| `keyword.operator` (+ arithmetic, bitwise, logical) | cyan | foreground | `base05` |
| `keyword.operator.assignment`, `.comparison` | cyan | foreground | `base05` |
| `constant.language` | cyan | orange | `base09` |
| `constant.other` | cyan | orange | `base09` |
| `constant.character.escape` | yellow | cyan | `base0C` |
| `entity.name.tag` | red | purple | `base08` |

## Contrast

Every text/background pair the theme produces was measured against WCAG AA
(4.5:1 for body text). This included resolving the bundled
`Merge.sublime-theme`'s own variables with this palette applied, because of the
naming problem below. 52 resolvable pairs, all now at or above 4.5:1; the
lowest is 4.53:1 (comments on the changed-word diff block).

### The naming problem

Meetio names its variables in camelCase — `hazardButtonBg`, `labelColor`,
`authorFg`. The bundled `Merge.sublime-theme` reads **snake_case** names that it
defines itself: `hazard_button_bg`, `label_color`, `author_fg`. The camelCase
names only take effect where Meetio's own rules reference them; everywhere else
the bundled defaults applied, and those are written for a light window.

The result was dark-gray-on-near-black in a lot of chrome: `author_fg`,
`disclosure_fg`, `icon_button_fg`, `preview_fg` and `hunk_button_fg` all
resolved to near-#000000 or #404040 against `#000018` — between 1.01:1 and
2.00:1. Several annotation badges had `fg` and `bg` resolving to the *same*
colour, i.e. 1.00:1 and invisible. This theme now defines both spellings, so
whichever path is live is correct.

Worth knowing if you fork this: `hazard_button_fg` in the bundled theme is the
literal string `"white"`, not a variable. White on the palette's `red`
(`#FF6B8F`) is 2.71:1, so `hazard_button_bg` is `red` at `l(- 32%)` — that
brings it to 6.05:1 without touching `red` itself, which is fine everywhere
else it's used.

### What moved for contrast

Three palette values were lightened. These are the only places the palette
deliberately departs from the Konsole/nvim source:

| | was | now | why |
| --- | --- | --- | --- |
| `comments` | `#71809C` | `#919CB2` | 3.14:1 on the removed-word diff block |
| `brown` | `#8189AE` | `#949ABA` | 3.65:1, same place |
| `violet` | `#7F5DD0` | `#A890DF` | 3.87:1 on the lightest panel |

`comments` matters most: at `#71809C` it passed on the window background
(5.20:1) and failed only inside diff blocks, which is precisely where comments
get read. The nvim config pins `base03` low on purpose so comments recede —
that intent survives, since `#919CB2` is still clearly dimmer than the `#ABB5C7`
body text.

Six alphas were raised, all of them text that had been washed out to
1.4–4.0:1:

| | was | now | result |
| --- | --- | --- | --- |
| `gutter_foreground` (line numbers) | `0.20` | `0.65` | 1.37 → 4.56:1 |
| `invalid` | `0.40` | `0.86` | 1.71 → 4.52:1 |
| `authorFg` / `author_fg` | `0.40` | `0.65` | 2.32 → 4.56:1 |
| `tabLabelColor` | `0.50` | `0.65` | 3.06 → 4.56:1 |
| `deprecated` | `0.63` | `0.70` | 3.82 → 4.52:1 |
| `helpLabelColor` | `0.60` | `0.65` | 4.01 → 4.56:1 |

Line numbers are the one judgement call: nvim's indent guides sit at 2.5:1 on
purpose, but gutter numbers are information you read rather than decoration, so
they get the text threshold.

## Transparency

Not possible. Sublime Merge has no transparency setting and no translucent
window background on Linux; the theme engine's `a()` alpha only blends layers
within an opaque window. KWin can force whole-window opacity via a window rule
on `sublime_merge`, but that fades the text too — unlike Konsole's `Opacity`,
which fades only the background.

## Files

```
theme/Merge Cyberpunk Signal.sublime-theme          window chrome, sidebar, graph
schemes/Cyberpunk Signal.sublime-color-scheme       diff + syntax colors
theme/{Commit Message,Diff,File Mode,Widget} - Merge Cyberpunk Signal.sublime-settings
                                                    point each widget at the scheme
```

Both palettes live in a `variables` block at the top of their file; everything
downstream is `var()`, so a re-skin is those two blocks.
