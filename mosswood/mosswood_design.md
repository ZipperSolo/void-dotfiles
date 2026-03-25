# Mosswood — Design Specification

A dark, green-focused terminal palette built for Wayland desktops.

---

## Color Palette

### Base & Surface

The background layer. Progresses from deepest black-green (Crust) to a mid-dark
green-grey (Surface 2). Use these for backgrounds, panels, borders, and layering.

| Name       | Hex       | Role                                      |
|------------|-----------|-------------------------------------------|
| Crust      | `#0a100e` | Deepest background, root window, bar bg   |
| Mantle     | `#0f1a16` | Secondary background, title bars, tooltips|
| Base       | `#141f1b` | Primary application/terminal background   |
| Surface 0  | `#1a2b25` | Input fields, buttons, elevated surfaces  |
| Surface 1  | `#213530` | Hover states, subtle borders              |
| Surface 2  | `#2a413a` | Active borders, stronger dividers         |

### Overlay

Mid-tones for muted UI chrome — inactive text, scrollbars, placeholder content.

| Name       | Hex       | Role                                      |
|------------|-----------|-------------------------------------------|
| Overlay 0  | `#3a5a50` | Muted/disabled text, inactive indicators  |
| Overlay 1  | `#4a6e62` | Comments, scrollbar hover                 |
| Overlay 2  | `#5a8274` | Section labels, secondary chrome          |

### Text

| Name       | Hex       | Role                                      |
|------------|-----------|-------------------------------------------|
| Subtext 0  | `#7da396` | Tertiary text, hints                      |
| Subtext 1  | `#98bfb1` | Secondary text, labels, module defaults   |
| Text       | `#c8e6da` | Primary foreground                        |

### Accent Greens

The signature colors. Green is the primary accent; the rest provide variety
without leaving the green family.

| Name       | Hex       | Role                                      |
|------------|-----------|-------------------------------------------|
| Green      | `#7eed9f` | **Primary accent** — focus rings, active indicators, caret, clock |
| Teal       | `#5ee4c0` | Secondary accent — audio, indicators      |
| Sage       | `#a3d9a5` | Strings in syntax highlighting            |
| Mint       | `#b4f0d1` | Light highlight, badges                   |
| Emerald    | `#3dd68c` | Success/OK states, battery charging       |
| Lime       | `#c3f76b` | Bright highlight, rarely used             |
| Pine       | `#2db87a` | Primary action buttons (bg on dark)       |
| Moss       | `#6abf7b` | Muted green accent                        |

### Extended (Non-Green)

Semantic and syntactic colors. Used sparingly to break the green monotone
for warnings, errors, and syntax highlighting categories.

| Name       | Hex       | Role                                      |
|------------|-----------|-------------------------------------------|
| Rose       | `#e8927c` | **Error/critical** — urgent workspaces, disconnected, critical temp |
| Gold       | `#e4c76a` | **Warning** — low battery, caution states |
| Lavender   | `#b4a6e8` | Keywords in syntax, CPU indicator         |
| Sky        | `#6ec4e8` | Functions in syntax, memory indicator     |
| Peach      | `#f0a882` | Numbers in syntax                         |
| Maroon     | `#d46a7e` | Reserved/alt-error                        |

---

## Design Principles

1. **Green dominant.** The eight accent greens carry the theme's identity.
   Non-green colors exist only for semantic meaning (error, warning) or
   syntax differentiation — never as decoration.

2. **Dark base with low contrast layering.** The six base/surface tones
   step gently from `#0a100e` to `#2a413a`. Layer depth through subtle
   background shifts, not borders (though thin 1–2 px borders in Surface 1/2
   are acceptable).

3. **Warm greens, cool darks.** Accents lean warm/yellow-green (Lime, Sage,
   Mint). Bases lean cool/blue-green (Crust through Surface 2). This
   temperature split creates natural separation between foreground and
   background.

4. **Two-pixel borders.** Window and panel borders are 2 px. Focus uses
   Green (`#7eed9f`), unfocused uses Crust/Base, urgent uses Rose.

5. **JetBrains Mono.** The monospace typeface throughout — terminal, bar,
   labels. DM Sans may be used for body text in documentation/web contexts.

---

## Semantic Mapping

### States

| State             | Color     | Hex       |
|-------------------|-----------|-----------|
| Focused / Active  | Green     | `#7eed9f` |
| Inactive          | Surface 1 | `#213530` |
| Unfocused         | Crust     | `#0a100e` |
| Urgent / Error    | Rose      | `#e8927c` |
| Warning           | Gold      | `#e4c76a` |
| Success / OK      | Emerald   | `#3dd68c` |
| Disabled / Muted  | Overlay 0 | `#3a5a50` |

### Syntax Highlighting

| Token      | Color    | Hex       |
|------------|----------|-----------|
| Keyword    | Lavender | `#b4a6e8` |
| Function   | Sky      | `#6ec4e8` |
| String     | Sage     | `#a3d9a5` |
| Number     | Peach    | `#f0a882` |
| Type       | Gold     | `#e4c76a` |
| Comment    | Overlay 1| `#4a6e62` |
| Error      | Rose     | `#e8927c` |
| OK/Success | Emerald  | `#3dd68c` |
| Prompt     | Green    | `#7eed9f` |
| Dim/Muted  | Overlay 0| `#3a5a50` |

---

## Application Recipes

### Sway / i3 (window manager)

```conf
# ── Mosswood palette ──
set $crust    #0a100e
set $mantle   #0f1a16
set $base     #141f1b
set $surface0 #1a2b25
set $surface1 #213530
set $surface2 #2a413a
set $green    #7eed9f
set $teal     #5ee4c0
set $text     #c8e6da
set $subtext  #98bfb1
set $overlay  #3a5a50
set $rose     #e8927c

# class                 border    bg        text      indicator  child_border
client.focused          $green    $surface0 $text     $teal      $green
client.focused_inactive $surface1 $base     $subtext  $surface1  $surface1
client.unfocused        $base     $crust    $overlay  $crust     $crust
client.urgent           $rose     $base     $text     $rose      $rose
```

### Waybar

```css
window#waybar       { background: #0a100e; color: #c8e6da; border-bottom: 2px solid #1a2b25; }
tooltip             { background: #141f1b; border: 1px solid #2a413a; color: #c8e6da; }
button.focused      { color: #7eed9f; border-bottom: 2px solid #7eed9f; }
button.urgent       { color: #e8927c; border-bottom: 2px solid #e8927c; }
#clock              { color: #7eed9f; }
#pulseaudio         { color: #5ee4c0; }
#cpu                { color: #b4a6e8; }
#memory             { color: #6ec4e8; }
```

### GTK (CSS for ReGreet / generic)

```css
window              { background-color: #0a100e; color: #c8e6da; }
entry               { background-color: #1a2b25; border: 1px solid #2a413a; caret-color: #7eed9f; }
entry:focus         { border-color: #7eed9f; }
button              { background-color: #1a2b25; border: 1px solid #2a413a; }
button:hover        { background-color: #213530; border-color: #7eed9f; }
button.suggested    { background-color: #2db87a; color: #0a100e; }
button.destructive  { color: #e8927c; border-color: #e8927c; }
```

### CSS Custom Properties

```css
:root {
  /* Base */
  --crust:     #0a100e;    --mantle:    #0f1a16;
  --base:      #141f1b;    --surface0:  #1a2b25;
  --surface1:  #213530;    --surface2:  #2a413a;
  /* Overlay */
  --overlay0:  #3a5a50;    --overlay1:  #4a6e62;
  --overlay2:  #5a8274;
  /* Text */
  --subtext0:  #7da396;    --subtext1:  #98bfb1;
  --text:      #c8e6da;
  /* Accent */
  --green:     #7eed9f;    --teal:      #5ee4c0;
  --sage:      #a3d9a5;    --mint:      #b4f0d1;
  --emerald:   #3dd68c;    --lime:      #c3f76b;
  --pine:      #2db87a;    --moss:      #6abf7b;
  /* Extended */
  --rose:      #e8927c;    --gold:      #e4c76a;
  --lavender:  #b4a6e8;    --sky:       #6ec4e8;
  --peach:     #f0a882;    --maroon:    #d46a7e;
}
```

### Terminal (generic 16-color mapping)

```
color0  (black)          #0a100e   Crust
color1  (red)            #e8927c   Rose
color2  (green)          #7eed9f   Green
color3  (yellow)         #e4c76a   Gold
color4  (blue)           #6ec4e8   Sky
color5  (magenta)        #b4a6e8   Lavender
color6  (cyan)           #5ee4c0   Teal
color7  (white)          #c8e6da   Text
color8  (bright black)   #3a5a50   Overlay 0
color9  (bright red)     #d46a7e   Maroon
color10 (bright green)   #3dd68c   Emerald
color11 (bright yellow)  #f0a882   Peach
color12 (bright blue)    #6ec4e8   Sky
color13 (bright magenta) #b4a6e8   Lavender
color14 (bright cyan)    #b4f0d1   Mint
color15 (bright white)   #c8e6da   Text
background               #141f1b   Base
foreground               #c8e6da   Text
cursor                   #7eed9f   Green
```
