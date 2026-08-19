# Cyberpunk Theme — Sublime Merge

A Sublime Merge theme matching my Konsole profile and Neovim colorscheme, so a
diff here reads like the same file in the editor.

Derived from [Meetio Theme](https://github.com/meetio-theme/merge-meetio-theme)
(Palenight, MIT), palette replaced and some scope rules re-pointed.

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

Both reload on save — no restart. On macOS the Packages dir is
`~/Library/Application Support/Sublime Merge/Packages`, on Windows
`%APPDATA%\Sublime Merge\Packages`.

## Palette

From two sources that turned out to hold the same colors:
`~/.local/share/konsole/cyberpunk-signal.colorscheme` and the
`base16-cerulean-signal-dark` overrides in
`~/.config/nvim/lua/plugins/colorscheme.lua`.

| variable | value | source |
| --- | --- | --- |
| `background` | `#000018` | `Background` / `base00` |
| `foreground` | `#ABB5C7` | `Foreground` / `base05` |
| `comments` | `#919CB2` | `Color0Intense` / `base03`, lightened |
| `blue` | `#81A0FF` | `Color4Intense` / `base0D` |
| `cyan` | `#3FE0D8` | `Color6Intense` |
| `accent` | `#27BAB5` | `Color6` / `base0C` |
| `green` | `#2DBE70` | `Color2` / `base0B` |
| `purple` | `#B192F9` | `Color5` / `base08` |
| `pink` | `#F182E8` | `Color5Intense` / `base0E` |
| `red` | `#FF6B8F` | `Color1Intense` |
| `orange` | `#4FB1E2` | `Color4` / `base09` — a blue, see below |
| `yellow` | `#D394B9` | `Color3` / `base0A` — a mauve, see below |
| `brown` | `#949ABA` | `ForegroundFaint` / `base0F`, lightened |
| `violet` | `#A890DF` | nvim `SnacksIndentScope`, lightened |
| `white` | `#E2E8F2` | `ForegroundIntense` |
| `cursor` | `#3DFF9A` | `Color2Intense` — chosen, nothing sets a caret |
| `diffAdded` | `#00FF95` | `Color2Intense` at full chroma |
| `diffDeleted` | `#DB0677` | chosen — see below |
| `diffModified` | `#4FB1E2` | `Color4` |

**No orange or yellow.** The nvim config drops both on purpose, so those two
variables hold a blue and a mauve. Tag badges and the unmerged icon are mauve
here, not yellow.

**`diffDeleted` is the one invented color.** The palette jumps straight from
magenta (hue 305°) to red (348°) with nothing between; `#DB0677` is 328°.

The accent ladder is `background +3/+6/+12%` lightness rather than upstream's
`+2/+4/+10%`, which was tuned for a background four times lighter.

## Diff blocks

| | color | line | char | text on char |
| --- | --- | --- | --- | --- |
| added | `#00FF95` | `0.16` → `#00292C` | `0.23` → `#003B35` | 6.1:1 |
| removed | `#DB0677` | `0.34` → `#4A0238` | `0.46` → `#650344` | 6.2:1 |

The two sides need different alphas because the bases differ in luminance —
`#DB0677` at the added side's `0.16` would be 1.10:1 against the page, i.e.
invisible. `diffAdded`/`diffDeleted` themselves stay bright since they also
drive the 2px gutter markers.

Twelve syntax scopes were re-pointed where Meetio's color for a token
disagreed with the base16 slot nvim uses for it — keywords and `storage.type`
to pink (`base0E`), symbol operators to plain foreground (`base05`), language
constants to `base09`, escapes to `base0C`, tags to `base08`.

## Contrast

Every text/background pair measures at or above WCAG AA 4.5:1, lowest 4.53:1
(comments on the changed-word block). `comments`, `brown` and `violet` were
lightened to get there, and six alphas raised — `gutter_foreground` most of
all, from 0.20 to 0.65.

**The gotcha if you fork this:** Meetio names variables in camelCase, but the
bundled `Merge.sublime-theme` reads **snake_case** names it defines itself. So
outside the rules Meetio writes, its light-theme defaults were applying —
`author_fg`, `disclosure_fg`, `icon_button_fg` and friends landed near
`#404040` on `#000018`, and some annotation badges had `fg` equal to `bg`. This
theme defines both spellings. Note `hazard_button_fg` is the literal string
`"white"`, so `hazard_button_bg` is `red` at `l(- 32%)` to make it legible.

## Transparency

Not possible. Sublime Merge has no transparency setting and no translucent
window background on Linux. KWin can force whole-window opacity with a rule on
`sublime_merge`, but that fades the text too, unlike Konsole's `Opacity`.

## Files

```
theme/Merge Cyberpunk Signal.sublime-theme       window chrome, sidebar, graph
schemes/Cyberpunk Signal.sublime-color-scheme    diff + syntax colors
theme/{Commit Message,Diff,File Mode,Widget} - Merge Cyberpunk Signal.sublime-settings
```

Both palettes live in a `variables` block at the top of their file; everything
downstream is `var()`, so a re-skin is those two blocks.
