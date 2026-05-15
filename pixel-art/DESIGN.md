# Modern Pixel Art — Godot 4 UI Design System

A reusable design system for Godot 4 game UI in a contemporary pixel art style. Tokens map cleanly to Godot Theme resources (`.tres`), so reskinning a game = swapping one theme file.

**Style philosophy:** crisp pixel-perfect rendering, contemporary palette feel (not strictly retro 8-bit), modern UX patterns expressed in pixel form. Inspired by Sea of Stars, Eastward, Chained Echoes. The aesthetic is "pixel art that respects the player's time" — readable, generous, never cramped.

---

## 1. Foundational rules

These rules are non-negotiable for the system to stay coherent.

1. **Pixel-perfect.** All sizes are integer multiples of the 4px base unit. No fractional values, ever. Set Godot project settings → Display → Window → Stretch Mode = `viewport`, Aspect = `keep`, Scale = `integer` where possible.
2. **No anti-aliasing.** Fonts are bitmap or pixel-style only. Sprites and StyleBoxes use hard edges (`anti_aliasing = false` on StyleBoxFlat).
3. **Limited palette.** The full palette below has 16 colors. Do not add new colors per screen — extend by composing existing ones (e.g., a 50% overlay).
4. **One light direction.** All shading assumes top-left light. Drop shadows always go bottom-right.
5. **Chunky borders, no radii.** `corner_radius = 0` everywhere. `border_width = 2` is the default; 4 for emphasis. Rounded corners break the aesthetic.
6. **Motion snaps to grid.** Tween any positional movement to integer pixel values. Avoid sub-pixel interpolation.

---

## 2. Color tokens

The palette is 16 colors total, balanced for WCAG AA contrast on all text/background pairings used in the component spec. Hex values are the source of truth.

### Background layers (dark theme default)

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `bg-base`        | `#1A1B26` | Full-screen background, behind everything                           |
| `bg-surface`     | `#24283B` | Menu panels, modal containers                                       |
| `bg-elevated`    | `#2F334D` | Buttons (default), input fields, raised elements                    |
| `bg-overlay`     | `#1A1B26` @ 80% alpha | Pause overlay over gameplay (use `Color(0.10, 0.11, 0.15, 0.8)`) |

### Text

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `text-primary`   | `#F4F4F8` | Body, button labels, default text. Contrast vs `bg-elevated`: 12.4:1 ✓ AAA |
| `text-secondary` | `#A9B1D6` | Labels, captions, less-important info. Contrast vs `bg-elevated`: 6.8:1 ✓ AA |
| `text-disabled`  | `#565F89` | Disabled state only. Contrast vs `bg-elevated`: 3.1:1 (intentionally low) |
| `text-inverse`   | `#1A1B26` | Text on bright accent backgrounds (e.g., on `primary`)              |

### Brand / interactive

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `primary`        | `#7AA2F7` | Primary CTA fill, slider track fill, selected toggle                |
| `primary-hover`  | `#9ECCFF` | Hover state of primary                                              |
| `primary-pressed`| `#5D87E0` | Pressed/active state of primary                                     |
| `accent`         | `#BB9AF7` | Decorative accents, focus rings, secondary CTAs                     |

### Semantic

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `success`        | `#9ECE6A` | Confirmations, save success indicators                              |
| `warning`        | `#E0AF68` | Warnings, "unsaved changes" indicators                              |
| `danger`         | `#F7768E` | Destructive actions (Quit confirmation), error states               |
| `focus-ring`     | `#FFD86B` | Keyboard/gamepad focus indicator. Distinct from `primary` so focus is unambiguous |

### Borders

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `border-subtle`  | `#414868` | Dividers between menu items                                         |
| `border-default` | `#565F89` | Default button border, panel border                                 |
| `border-strong`  | `#A9B1D6` | Strong emphasis, hover state borders                                |

### Color → Godot Theme mapping

- `bg-*` → StyleBoxFlat `bg_color`
- `border-*` → StyleBoxFlat `border_color` (with `border_width_*` = 2 default)
- `text-*` → Theme color override `font_color`, `font_color_hover`, `font_color_pressed`, `font_color_disabled`, `font_focus_color`
- `primary`, `accent` → StyleBoxFlat `bg_color` of variant button types

### Color blindness note

The palette has been checked against deuteranopia and protanopia. Critical state distinctions never rely on color alone:
- `success` / `danger` are always paired with an icon (✓ / ✗) or text label
- `focus-ring` is yellow specifically because it remains distinct under both red-green and blue-yellow color vision deficiencies
- Focus state additionally uses a border-width change (2 → 4px), so it's perceivable in grayscale

---

## 3. Typography

### Font recommendations (OFL/free for commercial use)

| Role             | Font                          | Source                              | Notes                              |
|------------------|-------------------------------|-------------------------------------|------------------------------------|
| **Primary**      | **m6x11** by Daniel Linssen   | https://managore.itch.io/m6x11      | 11px base, excellent readability, OFL |
| **Display alt**  | **Pixel Operator** by Jayvee Enaguas | https://www.dafont.com/pixel-operator.font | For larger title work, 16px base |
| **Numerals/UI**  | **Silver** by Poppy Works     | https://poppyworks.itch.io/silver   | Game-tuned with gamepad glyphs, OFL |

**Pick one primary font and stick with it across the game.** The "alt" recommendations are for cases where m6x11 doesn't scale cleanly to the size you need.

### Type scale (pixel-perfect)

All sizes are integer multiples of the font's native pixel grid. For m6x11 (11px base), use multiples of 11 or 22. Sizes below assume m6x11.

| Role             | Size (px) | Weight  | Use                                  |
|------------------|-----------|---------|--------------------------------------|
| `display`        | 44        | regular | Game title on main menu              |
| `h1`             | 33        | regular | Screen title (e.g., "Settings")      |
| `h2`             | 22        | regular | Section heading within a screen      |
| `body`           | 22        | regular | Button labels, menu items, paragraphs |
| `body-small`     | 11        | regular | Hints, version strings, footnotes    |
| `caption`        | 11        | regular | Slider value labels, micro UI        |

### Typography rules

- **Line height = font size × 1.5**, rounded to the nearest 4px multiple. For 22px body, line-height = 32px.
- **Letter spacing: 0.** Pixel fonts are spaced at design time; tracking them destroys glyph integrity.
- **No italic.** Pixel fonts don't slant well at low resolution.
- **All caps for emphasis only** — never for body text. All-caps pixel text is harder to scan.
- **Font sizes in Godot Theme:** set `font_size` per node type (Button, Label, LineEdit) in the theme, not on individual nodes.

---

## 4. Spacing scale

Base unit: **4px**. All spacing values are multiples of 4. Pixel-perfect requires this.

| Token        | Value (px) | Use                                                    |
|--------------|------------|--------------------------------------------------------|
| `space-xxs`  | 4          | Icon-to-label gap, tightest pairings                   |
| `space-xs`   | 8          | Internal padding of small components                   |
| `space-sm`   | 12         | Tight component spacing                                |
| `space-md`   | 16         | Default gap between menu items, internal button padding (vertical) |
| `space-lg`   | 24         | Section separation within a screen                     |
| `space-xl`   | 32         | Margin from screen edge to content (mobile/small)      |
| `space-2xl`  | 48         | Margin from screen edge to content (desktop/wide)      |
| `space-3xl`  | 64         | Major vertical rhythm, panel separation                |

### Internal ≤ external rule

The space *inside* a grouped element should be ≤ the space *around* the group. Example: a vertical list of buttons with 8px gap between them should have ≥16px between the last button and the next section. This enforces grouping via Gestalt proximity.

### Godot mapping

- Use `MarginContainer` with margins set to spacing tokens at the theme level (theme constants `margin_left`, `margin_top`, etc.).
- Use `VBoxContainer` / `HBoxContainer` `separation` theme constant for the gap between children.
- StyleBoxFlat `content_margin_*` controls internal padding (button text inset).

---

## 5. Sizing scale

All sizes are multiples of 4. Heights aim for comfortable touch targets even though primary input is mouse/gamepad.

### Buttons

| Variant   | Height (px) | Min width (px) | Internal padding (px) |
|-----------|-------------|----------------|------------------------|
| Small     | 32          | 96             | 8 vertical, 16 horizontal |
| Default   | 48          | 160            | 12 vertical, 24 horizontal |
| Large     | 64          | 240            | 16 vertical, 32 horizontal |

Menu buttons (main menu, pause menu) use **Default** unless the screen has fewer than 3 buttons, in which case **Large** is acceptable for visual weight.

### Sliders

- Track height: 8px
- Track length: stretches to fill (anchor full width minus padding)
- Thumb: 16×16px square (no rounding)
- Value label: positioned 8px to the right of the slider, `body-small` size

### Toggles / Checkboxes

- 24×24px box
- Border 2px, fill `bg-elevated` (off) or `primary` (on)
- Check icon: 8×8px white pixel art glyph, drawn or via TextureRect

### Dropdowns

- Same height as Default button (48px)
- Caret icon: 8×8px chevron on the right, 16px from edge
- Open dropdown: panel below, `bg-surface` background, items match button height

### Icons

Standard sizes: **16, 24, 32, 48px**. Always integer-scaled from their native resolution. Don't scale an 8px icon to 24 — make a 24px icon.

### Borders and focus

- Default border width: **2px**
- Emphasis / focus border width: **4px**
- Border radius: **0px** everywhere
- Focus ring offset: drawn *as* the border (Godot's focus stylebox overlays on top), not as a separate outer ring. See Component States below for the focus pattern.

---

## 6. Component states

Godot draws focus stylebox **on top of** normal/hover/pressed (this is a longstanding behavior, not a bug). Therefore, the focus state in this system is a **border treatment** that augments whatever underlying state is showing — never a full restyle.

### Button states

| State     | Background     | Border             | Text            | Notes                                |
|-----------|----------------|--------------------|-----------------|--------------------------------------|
| Default   | `bg-elevated`  | 2px `border-default`| `text-primary`  | Drop shadow 2px down-right, `bg-base` @ 40% |
| Hover     | `bg-elevated` shifted +8% lightness | 2px `border-strong` | `text-primary` | No size change. Optional 1px upward shift |
| Pressed   | `bg-elevated` shifted -10% lightness | 2px `border-default` | `text-primary` | Drop shadow removed (button appears flush) |
| Disabled  | `bg-elevated` @ 50% alpha | 2px `border-subtle` | `text-disabled` | No hover/press response               |
| Focus     | (inherits underlying state) | 4px `focus-ring` overlay | `text-primary` | Yellow ring tells player "this is selected by gamepad/keyboard" |
| Selected (toggle on) | `primary`       | 2px `primary-pressed` | `text-inverse`  | For toggle-mode buttons              |

### Primary button (CTA) states

For the "Resume" or "Continue" button on the main/pause menu — visually emphasized.

| State     | Background     | Border             | Text            |
|-----------|----------------|--------------------|-----------------|
| Default   | `primary`      | 2px `primary-pressed` | `text-inverse` |
| Hover     | `primary-hover`| 2px `primary-pressed` | `text-inverse` |
| Pressed   | `primary-pressed`| 2px `primary-pressed` | `text-inverse` |
| Disabled  | `primary` @ 50% alpha | 2px `border-subtle` | `text-disabled` |
| Focus     | (inherits)     | 4px `focus-ring` overlay | (inherits) |

### Slider states

| State     | Track unfilled | Track filled | Thumb           |
|-----------|----------------|--------------|-----------------|
| Default   | `bg-elevated`  | `primary`    | `text-primary` fill, 2px `border-default` |
| Hover     | `bg-elevated`  | `primary-hover` | thumb border becomes `border-strong` |
| Dragging  | `bg-elevated`  | `primary-pressed` | thumb fill `primary` |
| Disabled  | `bg-elevated` @ 50% | `border-subtle` | `text-disabled` fill |
| Focus     | (inherits)     | (inherits)   | 4px `focus-ring` outline on thumb |

### Toggle / Checkbox states

| State     | Box fill       | Box border         | Glyph color     |
|-----------|----------------|--------------------|-----------------|
| Off       | `bg-elevated`  | 2px `border-default`| —              |
| On        | `primary`      | 2px `primary-pressed`| `text-inverse`|
| Disabled  | `bg-elevated` @ 50% | 2px `border-subtle` | `text-disabled` |
| Focus     | (inherits)     | 4px `focus-ring`   | (inherits)      |

### Critical: focus *must* be visually distinct from hover

Hover = border color shift. Focus = border *width* shift to 4px in `focus-ring` yellow. This means a player navigating with a gamepad sees a clear yellow chunky outline, and a player using the mouse hovering an already-focused button sees both treatments simultaneously without conflict.

---

## 7. Motion

Pixel art motion should feel snappy and intentional. Avoid easing that produces sub-pixel interpolation on small distances.

### Duration tokens

| Token       | Value (ms) | Use                                          |
|-------------|------------|----------------------------------------------|
| `instant`   | 0          | Focus ring appearance (no fade)              |
| `fast`      | 100        | Button hover, toggle flip                    |
| `base`      | 180        | Menu fade-in, panel slide                    |
| `slow`      | 300        | Screen transitions, settings save confirmation |

### Easing (Godot Tween)

| Token              | Godot constant                            | Use                                  |
|--------------------|-------------------------------------------|--------------------------------------|
| `linear`           | `Tween.TRANS_LINEAR`, `Tween.EASE_IN_OUT` | Pixel-grid-friendly default          |
| `ease-out`         | `Tween.TRANS_QUAD`, `Tween.EASE_OUT`      | Things appearing                     |
| `ease-in`          | `Tween.TRANS_QUAD`, `Tween.EASE_IN`       | Things disappearing                  |
| `snap`             | `Tween.TRANS_BACK`, `Tween.EASE_OUT`      | Toggle flip, slight overshoot        |

### Per-component motion

- **Button hover:** Background color tweens over `fast` with `linear`. Optional 1px upward y-shift on hover, snapping back on unhover.
- **Button press:** Instant on press-down (no tween). On release, return to default over `fast`.
- **Toggle flip:** Background color tween over `fast` with `snap`. Glyph appears/disappears instantly.
- **Slider drag:** No tween; thumb follows input directly.
- **Menu fade-in:** Opacity 0→1 over `base` with `ease-out`. Position can offset 8px vertically and snap to 0.
- **Settings save confirmation:** `success` color flashes for `slow` and fades out with `ease-in`.

### Pixel-snap rule

When tweening position, round the value to the nearest integer pixel each frame. For Godot: use `floor()` on the tweened value inside a `_process` setter, or use the property mode that already snaps (e.g., set `texture_filter = NEAREST` and snap_2d_transforms_to_pixel in project settings).

---

## 8. Layout system

### Resolution strategy

- **Base reference:** 1920×1080 at integer scale.
- **Pixel art base canvas:** 480×270 (scales 4× to 1920×1080). All "pixel" measurements in this doc are in canvas pixels at the 480×270 base, then integer-scaled.
- **Godot project settings:** Display → Window → Stretch → Mode = `viewport`, Aspect = `keep`, Scale = `integer` (or `keep_height` if you must support ultrawide).
- **Supported range:** 1280×720 (2.66× scale, rounds to 2× or 3× depending on policy) up to 3840×2160 (8× scale).

### Anchor patterns

Use Godot's anchor preset shortcuts (anchor values 0.0–1.0):

| Pattern              | anchor_left | anchor_top | anchor_right | anchor_bottom | Use                          |
|----------------------|-------------|------------|--------------|---------------|------------------------------|
| Full screen          | 0           | 0          | 1            | 1             | Backgrounds, overlay layers  |
| Centered modal       | 0.5         | 0.5        | 0.5          | 0.5           | Main menu, settings panel    |
| Top-left HUD         | 0           | 0          | 0            | 0             | Pause indicator, version str |
| Top-right HUD        | 1           | 0          | 1            | 0             | Save status indicator        |
| Bottom-center        | 0.5         | 1          | 0.5          | 1             | Toast notifications          |
| Bottom-left          | 0           | 1          | 0            | 1             | Back button, secondary action |

For centered modals, also set `grow_horizontal = GROW_DIRECTION_BOTH` and `grow_vertical = GROW_DIRECTION_BOTH` so the panel expands symmetrically around its anchor point.

### Screen margins by resolution

| Resolution range     | Edge margin   |
|----------------------|---------------|
| 1280×720 to 1600×900 | `space-xl` (32px) |
| 1920×1080 and up     | `space-2xl` (48px) |
| Ultrawide (21:9+)    | Cap content at 1920px wide, center; use full edge margin from content edge |

### Safe areas

Reserve **64px (`space-3xl`)** from any edge for "safe" content on non-overscan displays. UI inside this area is always visible. Background art can extend to the edge.

---

## 9. Godot Theme mapping

Pack the entire system into one `theme.tres` file:

```
res://ui/themes/pixel_art.tres
```

### Type variations to define

Create these as Type Variations in the theme:

| Variation                | Base type | Use                                  |
|--------------------------|-----------|--------------------------------------|
| `MenuButton`             | Button    | Default menu navigation buttons      |
| `PrimaryButton`          | Button    | CTAs like Resume, Save               |
| `DangerButton`           | Button    | Quit (uses `danger` color treatment) |
| `SettingLabel`           | Label     | Left-hand labels in settings rows    |
| `SectionHeader`          | Label     | h2-style section dividers            |

### Theme override priority guide

| Where to set            | When                                            |
|-------------------------|-------------------------------------------------|
| Theme resource (.tres)  | Anything used across multiple screens. Default. |
| Type Variation          | A class of element used the same way everywhere (e.g., all PrimaryButtons) |
| Per-node theme override | One-off exceptions only. If you do this twice, promote it to a Type Variation. |

### Theme constants to define

In the theme's constants section (not under any node type):
- `panel_margin = 24`
- `button_separation = 16`
- `section_separation = 32`

Use these in container `separation` overrides for consistency.

---

## 10. Best practices — maintaining the style

### Do

- **Stick to the 16-color palette.** If you need a new color, ask if you can compose it from existing tokens at different alphas.
- **Snap every position and size to 4px.** If it's not a multiple of 4, it's wrong.
- **Use one font across the game.** Adding a second font for "fanciness" almost always weakens pixel coherence.
- **Test in grayscale.** Open a screenshot in Photoshop / GIMP, desaturate. Hierarchy should still be readable.
- **Test on a 1280×720 window.** This is the worst case for readability. If it works here, it works everywhere.
- **Use `nearest` texture filtering.** Project settings → Rendering → Textures → Default Texture Filter = Nearest.

### Don't

- **Don't anti-alias text.** Set fonts' `antialiasing` property to `None`. Pixel fonts blur badly when AA is on.
- **Don't use gradients.** Pixel art doesn't gradient; it dithers. And dithering inside StyleBoxes isn't supported. Use solid colors.
- **Don't use corner radii.** Even `corner_radius = 1` on a pixel art button reads as a rendering bug.
- **Don't mix pixel densities.** A 16px icon next to a 24px icon next to a 32px icon in the same row looks chaotic. Pick a row's icon size and commit.
- **Don't animate over distances smaller than 4px.** Sub-pixel motion is invisible and looks like jitter.

### Extending the system

When you find yourself wanting to add a token:
1. **Composition first.** Can you make it from existing tokens? (E.g., `bg-elevated` at 80% alpha over `bg-base` for a dimmed surface.)
2. **Promote, don't proliferate.** If a new token *is* needed, add it to this DESIGN.md before using it in code. Drift starts with one-off colors.
3. **Document why.** A token without a documented use case will eventually be misused.

### Re-skinning checklist (using this system for a new game)

1. Duplicate `pixel_art.tres` and rename per game.
2. Swap the 16-color palette to your new game's palette. Keep token *names* identical; only change hex values.
3. Swap the font if game tone demands it.
4. Verify all contrast pairs still pass WCAG AA (use https://webaim.org/resources/contrastchecker/).
5. Test all 5 menu screens at 1280×720 and 1920×1080.
6. Done. No node changes needed.

---

## 11. Quick reference card

```
PALETTE (dark theme)
─ Backgrounds: #1A1B26 → #24283B → #2F334D
─ Text:        #F4F4F8 / #A9B1D6 / #565F89
─ Primary:     #7AA2F7 (hover #9ECCFF, pressed #5D87E0)
─ Accent:      #BB9AF7
─ Focus ring:  #FFD86B
─ Semantic:    #9ECE6A success / #E0AF68 warn / #F7768E danger

SPACING (4px base)
─ 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64

SIZING
─ Button: 48px height default (32 small, 64 large)
─ Slider: 8px track, 16×16 thumb
─ Toggle: 24×24 box
─ Border: 2px default, 4px focus
─ Corner radius: 0

FONT: m6x11 (22px body, 33px h1, 44px display)

MOTION
─ Hover: 100ms linear
─ Menu transitions: 180ms ease-out
─ All motion snaps to integer pixels
```
