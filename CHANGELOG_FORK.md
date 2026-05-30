# Deere-Redo Fork Changelog

## Overview
This document details all differences between the **Deere-Redo** fork (located in `~/.mixxx/skins/deere-redo`) and the stock **Deere** skin (installed at `/usr/share/mixxx/skins/Deere`).

**Original Version:** 2.3.0.01
**Fork Version:** 2.3.9.04
**Last Updated:** 2026-05-13

---

## Stock Deere Additions Since Fork

Features integrated from stock Deere on 2026-02-10:

- **EffectChainPresetButton** - Preset selector button added to both collapsed (22x22) and expanded (24x24) views in `effect_unit_controls.xml`.
- **Loop Anchor button** - `loop_anchor` toggle added to `deck_loop_controls.xml` after the reloop button, using `ic_loop_anchor_start.svg` / `ic_loop_anchor_end.svg` icons.
- **Slip border outline** - `SlipBorderOutlineColor`, `SlipBorderTopOutlineSize` (10), `SlipBorderBottomOutlineSize` (10) added to `deck_waveform.xml`. Color defined as `WaveformSlipBorderColor` variable (#f08c00) in `skin.xml`.

Not yet incorporated:

- **Stereo VU meter singletons** - (deliberate divergence) Stock Deere defines `StereoVUMeter` singletons in `mixer_controls_4decks_left.xml`. Deere-redo uses dedicated per-channel VU templates instead.
- **Stems support** - `stem_channel.xml` and `stem_control.xml` added upstream; `deck.xml` and `deck_small.xml` modified. Requires layout integration work for deere-redo's deck structure.
- **`WCueButton` / `WPlayButton` widgets** - Upstream `deck_controls_row.xml` switched from template-based buttons to `CueButton` widget (with `EmitOnPressAndRelease`) and a new `left_right_display_play_2state_button.xml` for the play button. Behaviorally more correct cue handling. Deere-redo uses `deck_play_cue.xml` template instead.
- ~~`#spinBoxRecentDays` styling~~ — ported 2026-05-02
- ~~Library sidebar drag-hover style~~ — ported 2026-05-02
- **`knob_medium_optional_label.xml`** - New upstream template (used by stems). Not in deere-redo.

---

## Bug Fixes

### 2025-10-11
1. **Fixed Mac stylesheet path** (`skin.xml`): `skins:Deere/style-mac.qss` to `skin:style-mac.qss`
2. **Removed non-existent menu style references** (`skin.xml`): Linux/Windows menu stylesheet paths that didn't exist
3. **Added missing ObjectName** (`deck_play_cue.xml`): PlayIndicator widget now identifiable for QSS
4. **Fixed missing XML closing tag** (`deck_play_cue.xml`): `</Template>` tag
5. **Removed trailing whitespace** in `deck_play_cue.xml`, `deck_overview_row.xml`, `outro_cues.xml`, `deck_loop_controls.xml`

### 2026-05-02
12. **Left spacer on play/cue buttons** (`deck_controls_row.xml`): reverted to 1px (same as all other inter-control spacers).
13. **Main VU MinimumSize/MaximumSize corrected** (`vumeter_main.xml`): updated to `25,60`/`27,300` to account for 4px CSS padding on `#main_VuMeter_Group`.
14. **Fixed channel VU white-line artifact** (`mixer_column_channel_vu_left.xml`, `mixer_column_channel_vu_right.xml`): removed outer 1px spacers (redundant given `#channel_VuMeter_Group padding: 0 2px`) and tightened outer `MaximumSize` from 20 to 15px.
15. **Fixed PlayCueContainer minimum width** (`deck_play_cue.xml`): set `MinimumSize>32,42` (matching `deck_transport_controls.xml`) and changed inner stacked group policy from `min,me` to `me,me` — prevents buttons collapsing to zero width.
16. **Replaced hotcue count selection with cycling button** (`hotcues.xml`, `skin_settings.xml`): single `[Skin],hotcue_count` key (0–4) replaces the broken two-key boolean system. Five grids: 4 / 8 / 16 / 24 / 36 hotcues. Skin settings button cycles through all five options. Visibility uses `IsEqual` transform.
17. **Ported `#spinBoxRecentDays` styling** (`style.qss`): added to all `WBeatSpinBox` / `#spinBoxTransition` selectors; added `min-width: 55px`, `disabled` state.
18. **Ported library sidebar drag-hover** (`style.qss`): added `WLibrarySidebar[dragHover="true"]::item:hover` rule.

### 2026-05-11 – 2026-05-13 (pixel-spacing refinements)
19. **Narrowed PlayCueContainer max width** (`deck_play_cue.xml`): `MaximumSize` reduced from `60,-1` to `46,-1` — allows cue/play buttons to compress on 1080p screens.
20. **Capped HotcueGridContainer widths** (`hotcues.xml`): 24-hotcue container capped at 286 px max, 36-hotcue at 430 px — prevents unbounded growth on wide/HiDPI displays.
21. **LoopControlsSimple and LoopGrid sizing** (`deck_loop_controls.xml`): `MaximumSize` capped at natural content widths (118 px and 192 px respectively); inner spacers set to Fixed policy; 2 px inter-row gap between `LoopGridTop` and `LoopGridBottom`.
22. **BeatjumpContainer sizing** (`deck_beatjump_controls.xml`): `MaximumSize` width capped at 46 px; inner spacer policy set to Fixed; removed redundant zero-height spacer.
23. **ControlsRow spacing** (`style.qss`): `margin-top: 2px`; `padding-bottom: 1px` (reduces effective layout height to 47 px, giving 1 px internal slack + 1 px Deck border = 2 px visual gap below buttons); `AlignLeft | AlignTop` alignment (was `AlignVCenter`).
24. **OverviewRow spacing** (`style.qss`): `margin: 0` (was `margin: 2px 0 0 0`); `AlignLeft | AlignTop` — removes the 2 px external top gap and 1 px internal bottom slack.
25. **Loop/beatjump alignment** (`style.qss`): `AlignLeft | AlignTop` added to `#LoopControlsSimple`, `#LoopGrid`, `#LoopGridTop`, `#LoopGridBottom`, `#BeatjumpContainer` — pins button rows to top-left within each container.
26. **LoopGrid and BeatjumpContainer left margins** (`style.qss`): `margin-left: 1px` on `#LoopGrid` (1 px gap between `LoopControlsSimple` and the 16-beatloop grid) and `#BeatjumpContainer` (1 px gap before the spinbox).
27. **EffectUnitToggle** (`style.qss`): `margin-top: 1px`; `max-width: 46px` — 1 px external space above the button, slight width compression.
28. **EffectChainSelector flexible sizing** (`effect_unit_controls.xml`): both instances changed from `SizePolicy f,f` / `MinimumSize 110,22` / `MaximumSize 110,22` to `SizePolicy min,f` / `MinimumSize 80,22` / `MaximumSize 110,22` — chain preset dropdown now shrinks to 80 px when space is tight, matching `WEffectSelector` behaviour.

### 2026-05-01
8. **Capped PlayCueContainer max width** (`deck_play_cue.xml`): `MaximumSize` changed from `-1,-1` to `60,-1` — stops play/cue buttons expanding to fill available deck width.
9. **Capped channel VU meter column width** (`mixer_column_channel_vu_left.xml`, `mixer_column_channel_vu_right.xml`): added `MaximumSize>20,-1` to singleton outer group and `MixerStrip_4Decks` child; added `MaximumSize>15,-1` to `channel_VuMeter_Group` — prevents columns from expanding beyond 2×5px meter content.
10. **Capped main VU width** (`vumeter_main.xml`): added `MaximumSize>24,300` to `MainVu` — prevents stereo main meter growing wider than intended.
11. **Main VU padding** (`style.qss`): added `padding-left: 2px; padding-right: 2px;` to `#main_VuMeter_Group` for visual breathing room.

### 2026-02-09
6. **Fixed icon SVGs missing width/height attributes** (`icon/*.svg`): 31 SVG files lacked explicit `width` and `height` attributes, causing Qt to render them as invisible/zero-size. Replaced with stock Deere versions that include dimension attributes.
7. **Added 2 missing icon SVGs**: `ic_loop_anchor_start.svg` and `ic_loop_anchor_end.svg` copied from stock Deere.

---

## New Files Added (20 XML + 6 assets/docs)

### Templates
- **`beatjump_button.xml`** - Beatjump button template with 8px font and arrow indicators
- **`loop_button.xml`** - Loop control button template
- **`loop_move_button.xml`** - Loop move/shift button template
- **`cue_button_with_spacing.xml`** - Hotcue button with improved spacing
- **`deck_play_cue.xml`** - Play/cue controls extracted into standalone template
- **`deck_transport_controls.xml`** - Alternative transport control layout
- **`deck_loop_controls.xml`** - 16-button loop grid replacing `deck_looping_controls.xml`
- **`mixer_column_channel_vu.xml`** - Generic channel VU meter template
- **`mixer_column_channel_vu_left.xml`** - Left-aligned channel VU
- **`mixer_column_channel_vu_right.xml`** - Right-aligned channel VU
- **`mixer_column_volume_gain_deck1.xml`** - Deck 1 volume/gain (blue knobs)
- **`mixer_column_volume_gain_deck2.xml`** - Deck 2 volume/gain (carmine knobs)
- **`mixer_column_volume_gain_deck3.xml`** - Deck 3 volume/gain (purple knobs)
- **`mixer_column_volume_gain_deck4.xml`** - Deck 4 volume/gain (lime knobs)
- **`effect_rack_left.xml`** - Left-side effect rack (units 1 & 3)
- **`effect_rack_right.xml`** - Right-side effect rack (units 2 & 4)
- **`outro_cues.xml`** - Outro cue point controls
- **`special_cues_outro.xml`** - Special outro cue handling

### Assets and Documentation
- **`ic_track_color_48px.svg`** - Track color indicator icon
- **`ic_unfold_more_48px.svg`** - Expand/collapse icon
- **`knob-vertical.svg`** - Vertical knob graphic
- **`TEMPLATE_STRUCTURE_OUTLINE.txt`** - Template dependency documentation
- **`skin_include_tree.dot`** - GraphViz dependency graph source
- **`skin_include_tree.png`** - Visual dependency tree render

---

## Major Feature Changes

### 1. Extended Hotcue Grid (4 / 8 / 16 / 24 / 36 modes)
**File:** `hotcues.xml`
- Stock: 4 or 8 hotcues (toggle via `show_8_hotcues`)
- Fork: 4, 8, 16, 24, or 36 hotcue modes, selected by a single cycling config key
- Config key: `[Skin],hotcue_count` (0–4); a button in skin settings cycles through all five options
- Visibility: `IsEqual` transforms compare `hotcue_count` against each grid's index value
- Grid natural content widths (MaximumSize caps):

  | Index | Count | Layout | Max width |
  |-------|-------|--------|-----------|
  | 0     | 4     | 2×2    | 48 px     |
  | 1     | 8     | 2×4    | 96 px     |
  | 2     | 16    | 2×8    | 192 px    |
  | 3     | 24    | 2×12   | 286 px    |
  | 4     | 36    | 2×18   | 430 px    |

- Intro/outro cues separated into `special_cues.xml` and `outro_cues.xml`

### 2. Deck Overview Row Reorganization
**File:** `deck_overview_row.xml`
- Moved slip mode and track color buttons to right side of overview waveform
- Added `beats_translate_curpos` button (moved from stock's top row)
- Overview waveform centered between left controls (repeat, eject, quantize, keylock) and right controls (slip, track color, fx assign)

### 3. Deck Controls Row Simplification
**File:** `deck_controls_row.xml`
- Stock: 88-line inline definition with play/cue, hotcues, special cues, loops, beatjump
- Fork: 17-line template composition using extracted templates (`deck_play_cue.xml`, `special_cues.xml`, `hotcues.xml`, `outro_cues.xml`, `deck_loop_controls.xml`, `deck_beatjump_controls.xml`)

### 4. Waveform Color Variables
**File:** `skin.xml`, `deck_waveform.xml`, `library.xml`
- Stock: hardcoded colors (`#ffffff`, `#00FF00`, `#0000FF`, etc.)
- Fork: all waveform colors defined as variables in `skin.xml` and referenced via `<Variable>` tags
- Covers signal colors (high/mid/low), RGB mode colors, beat/playpos/loop/cue markers
- Overview waveform colors independently configurable from main waveform

### 5. Per-Deck Background Colors
**File:** `skin.xml`
- Stock: all decks use `#333333` background, `#b8000000` played overlay
- Fork: each deck has unique tinted background and overlay:
  - Deck 1 (blue): `#343940` bg, `#30000011` overlay
  - Deck 2 (orange): `#403c34` bg, `#30001111` overlay
  - Deck 3 (violet): `#403440` bg, `#30110000` overlay
  - Deck 4 (lime): `#3d4034` bg, `#30001100` overlay

### 6. Per-Deck Waveform Zoom Colors
**File:** `style.qss`
- Added deck-specific `WaveformZoomContainer` colors:
  - Deck 1: `#378DF7` (blue)
  - Deck 2: `#FFB108` (amber)
  - Deck 3: `#D700D7` (magenta)
  - Deck 4: `#88B31A` (lime)

### 7. Effect Rack Split Layout
**Files:** `effect_rack_left.xml`, `effect_rack_right.xml`, `main_decks.xml`
- Stock: single `effect_rack.xml` between main decks
- Fork: effect racks split to left/right gutters alongside deck gutters
- Effect unit controls layout changed from vertical to horizontal orientation
- Removed EffectChainPresetButton (see upstream delta section)

### 8. Mixer Changes
**Files:** `mixer_controls_4decks_left.xml`, `mixer_controls_4decks_right.xml`, `mixer.xml`
- Added channel mute buttons for gain stage (EQ kill button style, toggled via `[Skin],show_eq_kill_buttons`)
- Removed inline stereo VU meter singletons, replaced with dedicated VU meter templates
- Per-deck channel VU meters positioned outside the volume fader area
- Volume fader max width narrowed from 60 to 50

### 9. Beatgrid Controls Per-Deck Toggle
**File:** `deck_text_row.xml`
- Stock: global `[Skin],show_beatgrid_controls` toggle
- Fork: per-deck `[Skin],show_beatgrid_controls_deck<i>` toggle
- Undo button moved outside beatgrid controls section (always visible)
- BPM trigger area highlights when bpmlock is active

### 10. Additional Skin Settings
**File:** `skin_settings.xml`
- **Hotcue count** cycling button (`[Skin],hotcue_count`, 0–4 → 4 / 8 / 16 / 24 / 36 hotcues)
- **16 Beatjump/Loop** toggle (`[Skin],show_16_beatjump_loop`)
- **Beatgrid Controls** toggle (`[Skin],show_beatgrid_controls`)
- **Library Navbar** toggle (`[Skin],show_library_navbar`)
- Added icons to sampler add/settings buttons (was blank in stock)

### 11. Library Overview Waveform Colors
**File:** `library.xml`
- Added overview waveform color variables for library column waveforms
- Added `show_library_navbar` visibility connection

### 12. Stylesheet Changes
**File:** `style.qss` (1031 diff lines vs stock)
- Tighter padding throughout (controls row, text row, deck area)
- Beatjump/loop button styling (8px font, hover effects)
- Hotcue button spacing fixes
- Library selection color changed from `#006596` to `#FF6600`
- Sort indicator sizing (16x16 with padding)
- All `skin:/../Deere/` paths changed to `skin:` (standalone skin)
- All `skins:Deere/` template paths changed to `skin:` throughout XML files

---

## Modified Shared Files (47 XML files)

### Significant Changes
- **`skin.xml`** - Version bump, waveform color variables, per-deck backgrounds, `skin:` paths
- **`deck_controls_row.xml`** - Replaced 88-line inline layout with 17-line template composition
- **`deck_overview_row.xml`** - Reorganized button placement around overview waveform
- **`deck_text_row.xml`** - Per-deck beatgrid toggle, undo button always visible, BPM lock highlight
- **`deck_waveform.xml`** - Variable-based colors, minimum height 80px
- **`hotcues.xml`** - 4/12/24 hotcue modes
- **`effect_unit_controls.xml`** - Horizontal layout, removed EffectChainPresetButton
- **`mixer_controls_4decks_left/right.xml`** - Mute buttons, removed stereo VU singletons
- **`mixer.xml`** - Channel VU templates, removed SizePolicy constraint
- **`skin_settings.xml`** - New setting toggles, `skin:` paths, button icons
- **`special_cues.xml`** - Split into intro-only (outro in `outro_cues.xml`)
- **`main_decks.xml`** - Effect racks in gutters
- **`library.xml`** - Overview colors, navbar toggle
- **`tool_bar.xml`** - Disabled waveform height knob, variable-based FX button sizes

### Minor Changes (path fixes, sizing, SizePolicy tweaks)
- `beatloop_button.xml`, `deck.xml`, `deck_looping_controls.xml`, `deck_overview.xml`
- `deck_singletons.xml`, `deck_small.xml`, `deck_spinny.xml`
- `effect_meta_knob.xml`, `effect_parameter_button.xml`, `effect_parameter_knob.xml`
- `effect_rack.xml`, `effect_single_no_parameters.xml`, `effect_unit.xml`
- `effect_unit_no_parameters.xml`, `effect_unit_with_parameters.xml`
- `hotcue_button.xml`, `knob_toolbar.xml`, `mixer_column_main_vu_4decks.xml`
- `mixer_column_pfl_levels.xml`, `mixer_column_volume_gain.xml`
- `parallel_waveforms.xml`, `preview_deck.xml`, `sampler_controls_row.xml`
- `sampler_row.xml`, `sampler_rows_selection_button.xml`, `sampler_text_row.xml`
- `skinsettings_button.xml`, `skinsettings_category_button.xml`
- `skinsettings_sampler_buttons.xml`, `special_cue_button.xml`
- `vumeter.xml`, `vumeter_main.xml`, `vumeter_v.xml`

---

## Known Issues

- **Stereo VU meter singletons** not used; deere-redo uses per-channel VU templates instead (deliberate divergence)

---

## Credits

**Original Deere Skin:** RJ Ryan, S.Brandt
**Deere-Redo Fork:** Milkii (modifications, refactoring, bug fixes)
**License:** Creative Commons Attribution, Share-Alike 3.0 Unported

---

*Last regenerated: 2026-02-10*
