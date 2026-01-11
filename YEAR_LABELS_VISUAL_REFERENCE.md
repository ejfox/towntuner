# Year Labels - Visual Reference

This document provides ASCII art mockups and visual specifications for the year labels feature.

## Full Screen Layout

```
┌─────────────────────────────────────────────┐
│ ╔═══════════════════════════════════════╗  │
│ ║  TOWNTUNER         Prestige: I        ║  │
│ ║  [HIDE PLAYED]                        ║  │
│ ╚═══════════════════════════════════════╝  │
├─────────────────────────────────────────────┤
│                                             │
│  2012  [#001]  2012.07.04                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        [#002]  2012.07.20                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        [#003]  2012.07.30                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        [#004]  2012.08.04                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        ...                                  │
│        [#016]  2012.12.05                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│                                             │
│  2013  [#017]  2013.01.05                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        [#018]  2013.01.06                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        [#019]  2013.01.07                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        ...                                  │
│                                             │
│  2014  [#050]  2014.01.03                  │
│        Jeff Davis                           │
│        ──────────────────────────────       │
│        ...                                  │
│                                             │
│                                             │
│                   ┌─────────┐              │
│                   │  🎲     │              │
│                   │ SHUFFLE │              │
│                   └─────────┘              │
│                                             │
└─────────────────────────────────────────────┘
```

## Detail View: Year Label Anatomy

```
┌─ Screen Edge
│
├─ 16pt margin
│
│  ┌──── 44pt wide ────┐    ┌─── Episode Content ───┐
│  │                   │    │                        │
│  │  2012             │    │ [#001]  2012.07.04    │
│  │  ↑                │    │ Jeff Davis             │
│  │  14pt bold        │    │ ────────────────────   │
│  │  mono, 0.6 alpha  │    │                        │
│  │                   │    │                        │
│  └───────────────────┘    └────────────────────────┘
│          ↑                          ↑
│          └─ 12pt spacing ───────────┘
│
```

## Spacing & Alignment Grid

```
|<--16pt-->|<--44pt-->|<-12pt->|<---- Flexible ---->|
┌──────────┬──────────┬────────┬────────────────────┐
│   Edge   │   Year   │  Gap   │  Episode Content   │
│  Margin  │  Label   │        │                    │
└──────────┴──────────┴────────┴────────────────────┘

Example measurements:
- Screen edge to year label: 16pt
- Year label column width: 44pt
- Gap between year and episode: 12pt
- Episode content: remainder of screen width minus 16pt right margin
```

## Typography Specifications

```
Year Label: "2012"
├─ Font: SF Pro Monospaced
├─ Size: 14pt
├─ Weight: Bold (700)
├─ Color: White
└─ Opacity: 0.6 (60%)

Episode Number: "[#001]"
├─ Font: SF Pro Monospaced
├─ Size: 16pt (body)
├─ Weight: Regular (400)
├─ Color: White (or black when inverted)
└─ Opacity: 1.0 (or 0.5 when completed)

Episode Date: "2012.07.04"
├─ Font: SF Pro Monospaced
├─ Size: 13pt (caption)
├─ Weight: Regular (400)
└─ Color: White (or black when inverted)

Comptroller: "Jeff Davis"
├─ Font: SF Pro
├─ Size: 13pt (caption)
├─ Weight: Regular (400)
├─ Color: White
└─ Opacity: 0.7 (70%)
```

## State Variations

### Normal Episode (Unplayed)
```
│  2012  [#001]  2012.07.04    │
│        Jeff Davis             │
│        ──────────────────     │
```

### Completed Episode (Played)
```
│        [#002]  2012.07.20    │  <- 0.5 opacity
│        Jeff Davis             │  <- 0.5 opacity
│        ──────────────────     │  <- 0.5 opacity
```

### Currently Playing Episode (Inverted)
```
│  ████  ██████  ████████████  │  <- White background
│  ████  Jeff Davis            │  <- Black text
│  ████  ──────────────────    │  <- Black separator
```

### First Episode After Year Change
```
│  2013  [#017]  2013.01.05    │  <- Year label appears
│        Jeff Davis             │
│        ──────────────────     │
```

### Subsequent Episodes (Same Year)
```
│        [#018]  2013.01.06    │  <- No year label, empty space
│        Jeff Davis             │
│        ──────────────────     │
```

## With HIDE PLAYED Filter Active

### Before Filter (2012 with some played)
```
│  2012  [#001]  2012.07.04    │  <- Played (0.5 opacity)
│        Jeff Davis             │
│        ──────────────────     │
│        [#002]  2012.07.20    │  <- Played (0.5 opacity)
│        Jeff Davis             │
│        ──────────────────     │
│        [#003]  2012.07.30    │  <- Unplayed
│        Jeff Davis             │
│        ──────────────────     │
```

### After Filter (HIDE PLAYED = ON)
```
│  2012  [#003]  2012.07.30    │  <- Year label moves to first visible
│        Jeff Davis             │
│        ──────────────────     │
│        [#004]  2012.08.04    │  <- No year label
│        Jeff Davis             │
│        ──────────────────     │
```

### If All Episodes of a Year Are Hidden
```
│  (no episodes from 2012 visible)     │  <- Year completely skipped
│                                       │
│  2013  [#017]  2013.01.05            │  <- Next year with visible episodes
│        Jeff Davis                     │
│        ──────────────────────────     │
```
*Note: If all episodes from a year are filtered out, that year's label doesn't appear at all*

## Color Values (Hex)

```
Background:       #000000 (Black)
Primary Text:     #FFFFFF (White)
Year Label:       #FFFFFF @ 60% = #999999
Comptroller:      #FFFFFF @ 70% = #B3B3B3
Divider:          #FFFFFF @ 20% = #333333
Completed:        [All elements] @ 50% opacity
Inverted BG:      #FFFFFF (White)
Inverted Text:    #000000 (Black)
```

## Interactive States

### Year Label on Tap (Optional)
```
│  2012  [#001]  2012.07.04    │
│   ↑
│   └─ Opacity: 0.6 → 0.8 for 200ms
│      + Light haptic feedback
```

### Year Label Scroll Animation (Optional)
```
Frame 0:    2012 (opacity 0.3)   <- Entering viewport
Frame 1:    2012 (opacity 0.45)
Frame 2:    2012 (opacity 0.6)   <- Final state
Duration: 200ms ease-in-out
```

## Screen Size Adaptations

### iPhone SE (375pt width)
```
|<16>|<44>|<12>|<--- 287pt --->|<16>|
│ 2012  [#001]  2012.07.04     │
│       Jeff Davis              │
```

### iPhone Pro (390pt width)
```
|<16>|<44>|<12>|<--- 302pt --->|<16>|
│ 2012  [#001]  2012.07.04      │
│       Jeff Davis               │
```

### iPhone Pro Max (428pt width)
```
|<16>|<44>|<12>|<--- 340pt --->|<16>|
│ 2012  [#001]  2012.07.04         │
│       Jeff Davis                  │
```

## Landscape Orientation
```
┌────────────────────────────────────────────────────────────┐
│ TOWNTUNER                                    Prestige: I   │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ 2012  [#001]  2012.07.04  │  [#005]  2012.08.17  │       │
│       Jeff Davis          │        Jeff Davis     │       │
│ ──────────────────────────│──────────────────────│       │
│       [#002]  2012.07.20  │        [#006]  2012.08.24     │
│       Jeff Davis          │        Jeff Davis     │       │
│                           │                       │       │
```
*Note: Landscape might show multiple columns, year labels remain left-aligned*

## Year Jump Menu (Optional Feature)

```
┌─────────────────────────────────────┐
│  TOWNTUNER    📅  Prestige: I      │  <- Tap calendar icon
└─────────────────────────────────────┘
                  ↓
        ┌─────────────────┐
        │  Jump to Year   │
        ├─────────────────┤
        │  2012  (16)     │
        │  2013  (33)     │
        │  2014  (41)     │
        │  2015  (40)     │
        │  2016  (44)     │
        │  2017  (43)     │
        │  2018  (39)     │
        │  2019  (40)     │
        │  2021  (1)      │
        └─────────────────┘
         ↑       ↑
         Year    Episode count
```

## Accessibility View (VoiceOver)

```
Element: Year Label
├─ Role: Static Text / Button (if interactive)
├─ Label: "Year 2012"
├─ Hint: "First episode of 2012" (if interactive: "Tap to see year options")
└─ Traits: Header

Element: Episode Row
├─ Role: Button
├─ Label: "Episode 1, 2012-07-04, Jeff Davis, Unplayed"
├─ Hint: "Double tap to play"
└─ Traits: Button, [Dimmed if completed]
```

## Animation Timings

```
Year Label Fade In:     200ms ease-in-out
Year Label Tap:         200ms ease-in-out
Scroll to Year:         400ms ease-in-out
Highlight Pulse:        300ms ease-in-out

Timing Functions:
├─ Fade: .easeInOut(duration: 0.2)
├─ Scroll: .easeInOut(duration: 0.4)
└─ Pulse: .easeInOut(duration: 0.3)
```

## Z-Index Layering

```
Layer 5: Shuffle Button (floating)
Layer 4: Year Jump Menu (when open)
Layer 3: Episode Row (tapped/active)
Layer 2: Year Labels
Layer 1: Episode Content
Layer 0: Background (black)
```

## Testing Scenarios to Visualize

1. **Fresh Install**: All episodes unplayed, all years visible
2. **Mid-Prestige**: Some years complete (0.5 opacity), some in progress
3. **Filter Active**: Only unplayed episodes shown, year labels adjust
4. **Currently Playing**: One episode inverted, year label nearby
5. **First of Year**: Year label prominent on episode #1, #17, #50, etc.
6. **End of List**: 2021 with single episode, then shuffle button

## Comparison: Before vs After

### Before (No Year Labels)
```
│  [#001]  2012.07.04  │
│  Jeff Davis          │
│  ──────────────────  │
│  [#002]  2012.07.20  │
│  Jeff Davis          │
│  ──────────────────  │
│  ...                 │
│  [#017]  2013.01.05  │  <- Hard to spot year change
│  Jeff Davis          │
```

### After (With Year Labels)
```
│  2012  [#001]  2012.07.04  │
│        Jeff Davis          │
│        ──────────────────  │
│        [#002]  2012.07.20  │
│        Jeff Davis          │
│        ──────────────────  │
│        ...                 │
│  2013  [#017]  2013.01.05  │  <- Clear year transition
│        Jeff Davis          │
```

## Implementation Phases Visualized

### Phase 1: Basic Labels
```
│  2012  [#001]  2012.07.04  │  <- Just the label, no interaction
│        Jeff Davis          │
```

### Phase 2: + Scroll Navigation
```
│  TOWNTUNER    📅           │  <- Calendar icon added
│  2012  [#001]  2012.07.04  │  <- Can tap year or use menu
│        Jeff Davis          │
```

### Phase 3: + Animations
```
│  2012  [#001]  2012.07.04  │  <- Fade in, tap feedback, pulse
│   ↑↑↑  Jeff Davis          │
│   Animated on scroll        │
```

---

## Quick Reference Card

```
┌─────────────────────────────────────┐
│  YEAR LABELS CHEAT SHEET            │
├─────────────────────────────────────┤
│  Position:    Left side, 16pt edge  │
│  Width:       44pt column           │
│  Font:        14pt mono bold        │
│  Color:       White @ 60%           │
│  Shows:       First episode/year    │
│  Spacing:     12pt gap to content   │
│  Years:       2012-2019, 2021       │
│  Total:       9 year labels         │
└─────────────────────────────────────┘
```

This visual reference should help developers and designers understand exactly how the year labels should look and behave in the TownTuner episode list.
