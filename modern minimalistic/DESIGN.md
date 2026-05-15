Create a reusable design system for Godot 4 game UI in the 
[STYLE NAME] style. This system will be the source of truth for 
multiple game projects, with tokens mapping cleanly to Godot Theme 
resources (.tres) so re-skinning a game = swapping one theme file.

## Style Direction

--- Modern Minimalistic ---
- Clean geometry, flat or near-flat surfaces
- Restrained palette (2–4 colors + neutrals)
- Generous whitespace, strong typographic hierarchy
- Subtle motion, no decorative flourishes
- Reference vibe: Mini Metro, Alto's Odyssey, Monument Valley

## Required Tokens

### Color
Define named tokens with hex values and intended use:
- Background layers: bg-base, bg-surface, bg-overlay, bg-elevated
- Text: text-primary, text-secondary, text-disabled, text-inverse
- Brand: primary, primary-hover, primary-pressed, secondary, accent
- Semantic: success, warning, danger, info, focus-ring
- Borders/dividers: border-subtle, border-default, border-strong

Note which tokens map to Godot StyleBoxFlat properties 
(bg_color, border_color) vs Label font colors vs theme constants.

### Typography
- Font family recommendations (free/open-license, with fallbacks)
- For pixel art: specify pixel-perfect sizes only (e.g., 8/16/24px), 
  no fractional scaling
- Type scale: display, h1, h2, body, body-small, caption, button
- Weights, line heights, letter spacing per role

### Spacing
- Base unit (4px or 8px) and multipliers
- Named tokens: space-xs, space-sm, space-md, space-lg, space-xl
- For pixel art: all values must be integer multiples of the base unit

### Sizing
- Button heights (small/default/large)
- Input/slider track and thumb dimensions
- Border radii (or "no radius" for pixel art with chunky borders)
- Border widths
- Icon sizes
- Focus ring offset and width

### Component States
For every interactive component (button, slider, toggle, dropdown, 
text input), specify exact token changes for:
- default
- hover (mouse over)
- pressed (active)
- disabled
- focused (keyboard/gamepad — must be visually distinct from hover)
- selected (for toggles/radio states)

### Motion
- Duration tokens: instant, fast, base, slow (in ms)
- Easing curves named in Godot Tween terms 
  (TRANS_QUAD/EASE_OUT, TRANS_CUBIC/EASE_IN_OUT, etc.)
- What animates per component (button hover, panel transitions, 
  slider thumb, menu enter/exit)
- For pixel art: motion should snap to pixel grid where possible

## Layout System

- Target: 16:9, resolution-agnostic via anchor/margin (Godot Control 
  node conventions)
- Reference resolution: 1920x1080, but specify how every component 
  scales from 1280x720 to 4K
- Define anchor patterns: centered modal, full-screen overlay, 
  corner-anchored HUD-style
- Safe area considerations for ultrawide

## Best Practices Doc
Include a "maintaining the style" section:
- Do's and don'ts when adding new screens
- What breaks the aesthetic
- How to extend tokens without bloating the system
- Godot-specific: which properties to set via Theme vs per-node 
  override, and when each is appropriate

## Output
Organized doc with: token tables (color/type/spacing/sizing), 
component state matrices, motion spec, layout rules, Godot mapping 
notes, best practices.
