# Modern Minimalistic — Godot 4 UI Design System

A reusable design system for Godot 4 game UI in a contemporary minimalist style. Tokens map cleanly to Godot Theme resources (`.tres`), so reskinning a game = swapping one theme file.

**Style philosophy:** clean geometry, restrained palette, generous whitespace, strong typographic hierarchy. Subtle motion only. Inspired by Mini Metro, Alto's Odyssey, Monument Valley, and Dieter Rams' design principles. The aesthetic is "calm, confident, content-first" — nothing on screen unless it earns its place.

---

## 1. Foundational rules

These rules define the system's identity. Break one and the style breaks.

1. **Whitespace is a feature.** Empty space is the most-used design element. When in doubt, add more space, not more content.
2. **Hierarchy from typography first, color second.** A minimalist UI relies on type weight, size, and position to communicate importance — not bright colors or borders.
3. **Restrained palette.** Two neutrals + one brand color + semantic colors. Anything more breaks the aesthetic.
4. **One accent only.** Within a single screen, only one element gets the accent color. Multiple accents = no accent.
5. **Flat surfaces.** Drop shadows are allowed but extremely subtle (low opacity, large blur). No gradients in UI fills.
6. **Subtle motion.** Animations are short, eased, and never decorative. If a player notices the animation, it's too much.

---

## 2. Color tokens

The palette is intentionally tight: 12 tokens total. WCAG AA verified for all functional text/background pairings.

### Background layers (light theme — recommended default)

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `bg-base`        | `#FAFAFA` | Full-screen background                                              |
| `bg-surface`     | `#FFFFFF` | Cards, panels, raised surfaces                                      |
| `bg-elevated`    | `#F4F4F5` | Input fields, button rest state (subtle differentiation from base) |
| `bg-overlay`     | `#18181B` @ 60% alpha | Scrim behind modals (use `Color(0.094, 0.094, 0.106, 0.6)`) |

### Background layers (dark theme — alternative)

Swap these in for dark mode. Keep all other tokens the same; they're tuned to work in both.

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `bg-base`        | `#18181B` | Full-screen background                                              |
| `bg-surface`     | `#27272A` | Cards, panels                                                       |
| `bg-elevated`    | `#3F3F46` | Input fields, button rest state                                     |
| `bg-overlay`     | `#000000` @ 70% alpha |                                                  |

### Text (works in both themes via inversion)

#### Light theme:

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `text-primary`   | `#18181B` | Headlines, body, default text. Contrast vs `bg-base`: 16.9:1 ✓ AAA |
| `text-secondary` | `#52525B` | Labels, captions, supporting copy. Contrast: 7.5:1 ✓ AAA           |
| `text-disabled`  | `#A1A1AA` | Disabled state. Contrast: 2.8:1 (intentionally low)                |
| `text-inverse`   | `#FAFAFA` | Text on dark accent backgrounds                                     |

#### Dark theme: invert (`text-primary` = `#FAFAFA`, `text-secondary` = `#A1A1AA`, etc.)

### Brand / interactive

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `primary`        | `#3B82F6` | Primary CTAs, active toggles, slider fill. Single brand color.     |
| `primary-hover`  | `#60A5FA` | Hover state of primary                                              |
| `primary-pressed`| `#2563EB` | Pressed state of primary                                            |

> **Note on accent choice:** Blue (`#3B82F6`) is the recommended default — neutral, accessible across themes, universally readable. When reskinning a game, this is the *one* token most likely to change to match the game's mood (e.g., teal for cozy, amber for adventure, magenta for stylized).

### Semantic

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `success`        | `#10B981` | Save success, confirmations                                         |
| `warning`        | `#F59E0B` | "Unsaved changes" indicators                                        |
| `danger`         | `#EF4444` | Destructive actions (Quit confirmation), errors                     |
| `focus-ring`     | `#3B82F6` | Same as `primary`. Focus = subtle 2px outline ring offset 2px.      |

Semantic colors are used sparingly — usually only in transient feedback (toasts, save status), not as persistent UI elements.

### Borders / dividers

| Token            | Hex       | Use                                                                 |
|------------------|-----------|---------------------------------------------------------------------|
| `border-subtle`  | `#E4E4E7` (light) / `#3F3F46` (dark) | Hairline dividers between rows |
| `border-default` | `#D4D4D8` (light) / `#52525B` (dark) | Input fields, default button outline (when used) |

> Borders are used sparingly. Minimalist hierarchy prefers spacing and contrast over lines.

### Color blindness note

Critical state distinctions never rely on color alone:
- `success` (green) / `danger` (red) always paired with icon or text label
- `focus-ring` is the same hue as `primary` but uses a *ring* (visible regardless of color perception)
- Test in grayscale: the design must still be fully usable when desaturated

### Color → Godot Theme mapping

- `bg-*` → StyleBoxFlat `bg_color`
- `border-*` → StyleBoxFlat `border_color`, `border_width` typically 1px
- `text-*` → Theme `font_color`, `font_color_hover`, `font_color_pressed`, `font_color_disabled`, `font_focus_color`
- `primary` → StyleBoxFlat `bg_color` of `PrimaryButton` variant

---

## 3. Typography

Typography carries 80% of the hierarchy in this style. Choose the font carefully.

### Font recommendations (free, OFL/SIL)

| Role             | Font                  | Source                              | Notes                              |
|------------------|-----------------------|-------------------------------------|------------------------------------|
| **Primary**      | **Inter**             | https://rsms.me/inter/              | Designed for UI, exceptional readability at all sizes, OFL |
| **Alt (modern)** | **Geist**             | https://vercel.com/font             | Slightly more geometric/contemporary, OFL |
| **Alt (warm)**   | **IBM Plex Sans**     | Google Fonts                        | More humanist feel, OFL            |
| **Display only** | **Space Grotesk**     | Google Fonts                        | Reserve for game title only, OFL  |

**Pick one primary, max one display.** Mixing more typefaces is the fastest way to break minimalist coherence.

### Type scale

Uses a 1.25 ratio (Major Third) — provides clear hierarchy without dramatic jumps. Base size: 16px.

| Role             | Size (px) | Weight   | Line height | Use                              |
|------------------|-----------|----------|-------------|----------------------------------|
| `display`        | 48        | 600 SemiBold | 56 (1.17)   | Game title on main menu          |
| `h1`             | 32        | 600 SemiBold | 40 (1.25)   | Screen title (e.g., "Settings")  |
| `h2`             | 24        | 500 Medium   | 32 (1.33)   | Section headings                 |
| `body-large`     | 18        | 400 Regular  | 28 (1.55)   | Button labels in main menu       |
| `body`           | 16        | 400 Regular  | 24 (1.5)    | Default text, settings rows      |
| `body-small`     | 14        | 400 Regular  | 20 (1.43)   | Slider values, helper text       |
| `caption`        | 12        | 500 Medium   | 16 (1.33)   | Labels, badges, version strings  |

### Typography rules

- **Line height multiples of 4.** Aligns with spacing grid.
- **Letter spacing:**
  - Display / h1: `-0.02em` (tighter — large text reads loose by default)
  - h2: `-0.01em`
  - body / body-large: `0`
  - body-small / caption: `+0.01em` (slightly more open for legibility)
- **Use weight, not color, for emphasis.** A 600-weight word in primary text stands out more cleanly than a colored word.
- **Title case for headings, sentence case for body.** Avoid ALL CAPS except for caption-sized labels.

### Font sizes in Godot Theme

Set `font_size` per Type Variation in the theme — never override per-node unless one-off.

---

## 4. Spacing scale

Base unit: **8px**. Industry-standard 8-point grid system. All spacing values are multiples of 8 (with half-step of 4 only for tight icon/label gaps).

| Token        | Value (px) | Use                                                    |
|--------------|------------|--------------------------------------------------------|
| `space-xxs`  | 4          | Icon-to-label gap (half-step exception)                |
| `space-xs`   | 8          | Internal padding of compact components                 |
| `space-sm`   | 16         | Default internal padding, tight component gaps         |
| `space-md`   | 24         | Default gap between menu items, between form rows      |
| `space-lg`   | 32         | Section separation within a screen                     |
| `space-xl`   | 48         | Major content block separation                         |
| `space-2xl`  | 64         | Screen edge margin (desktop)                           |
| `space-3xl`  | 96         | Generous panel separation, hero margins                |
| `space-4xl`  | 128        | Display-level whitespace (e.g., above main menu title) |

### The whitespace rule

In minimalism, **whitespace is the design**. Default to more space than feels comfortable. Specifically:
- Between menu items: `space-md` (24px) minimum
- Around a button group: `space-lg` (32px)
- From screen edge to content: `space-2xl` (64px) on desktop
- Between major sections: `space-xl` (48px)

### Internal ≤ external rule

The space *inside* a group should be ≤ the space *around* it. Example: settings rows with 16px internal padding need ≥24px gap to the next section.

### Godot mapping

- `MarginContainer` for outer margins (set theme constants `margin_left`, etc.)
- `VBoxContainer`/`HBoxContainer` `separation` theme constant for gaps
- StyleBoxFlat `content_margin_*` for internal padding

---

## 5. Sizing scale

All multiples of 8 (with 4 as half-step for fine-tuning).

### Buttons

| Variant   | Height (px) | Min width (px) | Internal padding (px)        |
|-----------|-------------|----------------|------------------------------|
| Small     | 32          | 80             | 8 vertical, 16 horizontal    |
| Default   | 40          | 120            | 8 vertical, 20 horizontal    |
| Large     | 48          | 160            | 12 vertical, 24 horizontal   |

Menu buttons use **Large**. In-screen actions use **Default**. Compact UI uses **Small**.

### Sliders

- Track height: 4px
- Track length: stretches to fill
- Thumb: 20×20px circle (`border-radius` = 10px)
- Value label: 16px to the right of the slider, `body-small` size

### Toggles / Switches

Switches (toggle pill shape, not checkbox) suit this style better:
- 44×24px pill, fully rounded (radius 12px)
- Knob: 20×20 circle with 2px inset
- Off: `bg-elevated` track, `border-default` border, knob `text-secondary`
- On: `primary` track, no border, knob `text-inverse`

If using checkbox style instead:
- 20×20px box, 4px corner radius
- Border 1.5px `border-default`, fill `bg-surface` (off) or `primary` (on)

### Dropdowns

- Height matches button Default (40px)
- Chevron icon: 16×16 on the right, 12px from edge
- Border 1px `border-default`, 4px corner radius
- Open: panel below with `bg-surface`, 1px border, subtle shadow, items 40px tall

### Icons

Standard sizes: **16, 20, 24, 32px**. Use line-style icons (1.5–2px stroke), not filled, for consistency with the airy aesthetic. Recommended set: Lucide Icons (https://lucide.dev — open source, MIT).

### Borders, corners, shadows

- Border width: **1px** (default), **1.5px** (input focus state)
- Corner radius: **4px** (small components like checkboxes), **6px** (buttons, inputs), **8px** (panels, cards)
- Shadow (when used — sparingly):
  - `shadow-sm` (cards): `Color(0, 0, 0, 0.04)`, blur 4px, y-offset 2px
  - `shadow-md` (modals): `Color(0, 0, 0, 0.08)`, blur 16px, y-offset 4px
  - Godot's StyleBoxFlat supports `shadow_color`, `shadow_size`, `shadow_offset` natively

---

## 6. Component states

Godot draws focus stylebox **on top of** other states. In this system, focus is a **subtle outer ring** (2px offset 2px outside the element), so it overlays naturally without conflicting with the underlying state.

### Default button states

| State     | Background      | Border               | Text             | Shadow |
|-----------|-----------------|----------------------|------------------|--------|
| Default   | `bg-elevated`   | none                 | `text-primary`   | none   |
| Hover     | `bg-elevated` darkened 4% | none      | `text-primary`   | `shadow-sm` (subtle lift) |
| Pressed   | `bg-elevated` darkened 8% | none      | `text-primary`   | none   |
| Disabled  | `bg-elevated` @ 60% | none             | `text-disabled`  | none   |
| Focus     | (inherits)      | 2px `focus-ring` ring offset 2px outside | (inherits) | (inherits) |

### Primary button (CTA) states

| State     | Background        | Border  | Text            |
|-----------|-------------------|---------|-----------------|
| Default   | `primary`         | none    | `text-inverse`  |
| Hover     | `primary-hover`   | none    | `text-inverse`  |
| Pressed   | `primary-pressed` | none    | `text-inverse`  |
| Disabled  | `primary` @ 50%   | none    | `text-disabled` |
| Focus     | (inherits)        | 2px `focus-ring` outer ring offset 2px | (inherits) |

### Slider states

| State     | Track unfilled  | Track filled     | Thumb                                  |
|-----------|-----------------|------------------|----------------------------------------|
| Default   | `bg-elevated`   | `primary`        | `bg-surface` fill, 1.5px `border-default` |
| Hover     | `bg-elevated`   | `primary-hover`  | thumb border `text-secondary`          |
| Dragging  | `bg-elevated`   | `primary-pressed`| thumb fill `primary`, border same      |
| Disabled  | `bg-elevated` @ 60% | `border-default` | `bg-surface`, border `border-subtle` |
| Focus     | (inherits)      | (inherits)       | 2px `focus-ring` outer ring on thumb   |

### Switch states

| State     | Track            | Knob            |
|-----------|------------------|-----------------|
| Off       | `bg-elevated` + 1px `border-default` | `text-secondary` |
| On        | `primary`        | `text-inverse`  |
| Disabled  | `bg-elevated` @ 60% | `text-disabled` |
| Focus     | (inherits)       | 2px `focus-ring` outer ring on track |

### Input field states

| State     | Background    | Border                  | Text            |
|-----------|---------------|-------------------------|-----------------|
| Default   | `bg-surface`  | 1px `border-default`    | `text-primary`  |
| Hover     | `bg-surface`  | 1px `text-secondary`    | `text-primary`  |
| Focus     | `bg-surface`  | 1.5px `primary`         | `text-primary`  |
| Disabled  | `bg-elevated` | 1px `border-subtle`     | `text-disabled` |
| Error     | `bg-surface`  | 1.5px `danger`          | `text-primary`  |

> Inputs are the exception where focus *replaces* the default border (not overlays). This is because the input's "selected state" is genuinely different — text entry mode — not just a navigation cursor.

### Focus must be distinct from hover

- Hover = background fill change (subtle darken)
- Focus = outer 2px ring in `primary` color, offset 2px outside the element bounds

This means a gamepad user sees a clear ring; a mouse user hovering over an already-focused button sees both treatments simultaneously without visual conflict.

---

## 7. Motion

Minimalist motion: short, eased, purposeful. Never decorative.

### Duration tokens

| Token       | Value (ms) | Use                                          |
|-------------|------------|----------------------------------------------|
| `instant`   | 0          | Focus ring appearance (no fade)              |
| `fast`      | 120        | Button hover, toggle switch                  |
| `base`      | 200        | Menu fade-in, panel transitions              |
| `slow`      | 320        | Screen transitions, expanded states          |

### Easing (Godot Tween)

| Token              | Godot constant                            | Use                                  |
|--------------------|-------------------------------------------|--------------------------------------|
| `ease-out`         | `Tween.TRANS_CUBIC`, `Tween.EASE_OUT`     | Default for entering elements        |
| `ease-in`          | `Tween.TRANS_CUBIC`, `Tween.EASE_IN`      | Elements leaving                     |
| `ease-in-out`      | `Tween.TRANS_CUBIC`, `Tween.EASE_IN_OUT`  | Two-direction transitions (drawer, etc.) |

> Cubic over Quad here: smoother, more "considered" feel that suits the aesthetic. Avoid Back/Elastic — they read as playful, which conflicts with minimalism.

### Per-component motion

- **Button hover:** Background color tween over `fast` with `ease-out`. Optional shadow fade-in.
- **Button press:** Background tween over `fast`; no scale/translate. The minimalist style does not "bounce."
- **Switch toggle:** Track color tween over `base`. Knob position tween over `base` with `ease-out`.
- **Slider drag:** No tween; thumb follows input directly.
- **Menu fade-in:** Opacity 0→1 + 12px y-translate over `base` with `ease-out`. Stagger child elements by 30ms each (subtle).
- **Modal open:** Scrim fade in over `base`. Modal panel scale from 0.97 → 1.0 + opacity 0 → 1 over `base` with `ease-out`.
- **Settings save toast:** Slide in from bottom (24px) + fade in over `base`. Hold `slow` * 4 (~1280ms). Fade out over `base`.

### Motion accessibility

Respect "reduced motion" preferences. In Godot, expose a `reduced_motion` user setting; when true:
- Skip translation/scale animations
- Keep opacity fades but halve their duration
- Skip any staggered timing

---

## 8. Layout system

### Resolution strategy

- **Base reference:** 1920×1080
- **Godot project settings:** Display → Window → Stretch → Mode = `canvas_items`, Aspect = `expand`. This lets UI scale crisply (vector fonts) while content reflows.
- **Supported range:** 1280×720 up to 4K, plus ultrawide.

### Anchor patterns

Standard Godot anchor presets (0.0–1.0):

| Pattern              | anchor_left | anchor_top | anchor_right | anchor_bottom | Use                          |
|----------------------|-------------|------------|--------------|---------------|------------------------------|
| Full screen          | 0           | 0          | 1            | 1             | Background layers            |
| Centered modal       | 0.5         | 0.5        | 0.5          | 0.5           | Settings panel, dialogs      |
| Left-aligned content | 0           | 0          | 0            | 1             | Main menu (left-aligned title and buttons) |
| Top-left HUD         | 0           | 0          | 0            | 0             | Pause indicator              |
| Top-right HUD        | 1           | 0          | 1            | 0             | Save status, settings gear   |
| Bottom-center        | 0.5         | 1          | 0.5          | 1             | Toasts                       |
| Bottom-left          | 0           | 1          | 0            | 1             | Back button, version label   |

For centered modals: `grow_horizontal = GROW_DIRECTION_BOTH`, `grow_vertical = GROW_DIRECTION_BOTH`.

### Screen margins by resolution

| Resolution           | Edge margin       |
|----------------------|-------------------|
| 1280×720 to 1600×900 | `space-xl` (48px) |
| 1920×1080+           | `space-2xl` (64px)|
| 2560×1440 / 4K       | `space-3xl` (96px)|
| Ultrawide (21:9+)    | Cap content width at 1440px wide, center; use generous edge margin from content |

### Safe areas

Reserve **64px** from any edge for "safe" UI on non-overscan displays. Backgrounds and atmospheric art can extend to the edge.

### Layout style: left-aligned, not centered

Centered menus feel generic and slightly retro. **Left-aligned content** with a strong vertical baseline at `space-2xl` from the left edge feels modern and intentional. Place:
- Title in the top-left area (anchor top-left + 64px from edges)
- Menu buttons stacked below, all sharing the same left edge
- Decorative element (subtle illustration, game world preview, abstract shape) takes the right half

This is a hallmark of contemporary minimalist game UI (Mini Metro, Alto's Odyssey, Monument Valley).

---

## 9. Godot Theme mapping

Pack everything into one theme file:

```
res://ui/themes/minimalistic_light.tres
res://ui/themes/minimalistic_dark.tres  (optional companion)
```

### Type variations

| Variation          | Base type | Use                                          |
|--------------------|-----------|----------------------------------------------|
| `MenuButton`       | Button    | Default menu navigation (transparent, text-only) |
| `PrimaryButton`    | Button    | CTAs (Resume, Save) — uses `primary` color   |
| `DangerButton`     | Button    | Quit (uses `danger` color, only on confirm dialogs) |
| `SettingRow`       | HBoxContainer | Label-left, control-right row layout    |
| `SettingLabel`     | Label     | Left-aligned label in settings rows          |
| `SectionHeader`    | Label     | h2-style section dividers                    |

> **Menu buttons in this style are transparent by default** — no border, no fill in rest state, just the label. The button reveals itself on hover (subtle background fill) and via focus ring. This is core to the minimalist feel; if you change this, you change the style.

### Theme constants

In the theme's constants:
- `panel_margin = 32`
- `button_separation = 24`
- `section_separation = 48`
- `row_separation = 16`

### Theme override priority

| Where                  | When                                              |
|------------------------|---------------------------------------------------|
| Theme resource         | Anything reused. Default location.                |
| Type Variation         | Class of element with consistent treatment        |
| Per-node theme override| Genuine one-offs only. Promote to variation if used twice. |

---

## 10. Best practices — maintaining the style

### Do

- **Embrace whitespace.** Default to more space than feels comfortable. Minimalism's superpower is restraint.
- **Use weight and size for hierarchy, not color.** A 600-weight headline at 32px next to 400-weight 16px body is more "minimalist" than a colored heading.
- **Single accent rule.** One element per screen gets the primary blue. The rest is neutral.
- **Align everything to the 8px grid.** This invisible consistency is what makes it feel "designed."
- **Test in grayscale.** If hierarchy survives desaturation, it's working.
- **Use Lucide icons (or similar consistent line-icon set).** Line icons with consistent stroke widths.

### Don't

- **Don't add a third typeface.** Two is the limit (body + display). Three is chaos.
- **Don't use multiple accent colors.** One brand color. Period. Semantic colors don't count — they're for transient feedback.
- **Don't add decorative borders.** If you're outlining a card, ask if you can use background contrast or spacing instead.
- **Don't use gradients in UI.** Solid colors only. Gradients are for game art, not interface.
- **Don't center-align everything.** It looks lazy and generic. Use left-alignment for content, center only for true modals.
- **Don't animate scale or translate when fade alone would do.** Subtle is the brand.

### Extending the system

When you want to add a token:
1. **Composition first.** Most needs are satisfied by combining existing tokens (e.g., `bg-elevated` at 50% alpha for a divider).
2. **Question the need.** Minimalism is about *removing*. If you're adding a token, ask whether the problem is actually a missing token or a missing constraint.
3. **Document why and where.** If a new token is approved, add it to this DESIGN.md with a documented use case.

### Re-skinning checklist (using this system for a new game)

1. Duplicate the theme `.tres` and rename per game.
2. **Most common change: just swap `primary`/`primary-hover`/`primary-pressed`.** That alone re-brands the UI without touching anything else.
3. Optionally swap the typeface (Inter → Geist → IBM Plex Sans). Keep weights identical.
4. Verify all WCAG AA contrast pairs still pass (https://webaim.org/resources/contrastchecker/).
5. Test all 5 menu screens at 1280×720, 1920×1080, and one ultrawide resolution.
6. Done.

---

## 11. Quick reference card

```
PALETTE (light theme default)
─ Backgrounds: #FAFAFA → #FFFFFF → #F4F4F5
─ Text:        #18181B / #52525B / #A1A1AA
─ Primary:     #3B82F6 (hover #60A5FA, pressed #2563EB)
─ Borders:     #E4E4E7 subtle / #D4D4D8 default
─ Semantic:    #10B981 success / #F59E0B warn / #EF4444 danger

SPACING (8px base)
─ 4 / 8 / 16 / 24 / 32 / 48 / 64 / 96 / 128

SIZING
─ Button: 40px height default (32 small, 48 large)
─ Slider: 4px track, 20×20 thumb (round)
─ Switch: 44×24 pill, 20×20 knob
─ Border: 1px default, 1.5px focused input
─ Corner radius: 4 / 6 / 8

FONT: Inter (16px body, 32px h1, 48px display)
─ Weights: 400 Regular, 500 Medium, 600 SemiBold

MOTION
─ Hover: 120ms ease-out
─ Menu transitions: 200ms ease-out
─ Respect reduced-motion preference
```
