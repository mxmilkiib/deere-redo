# Channel Mute Button Analysis: deere-redo vs Original Deere

## Summary
The deere-redo theme adds a **channel mute button** next to the gain knob in the mixer column, which is **not present** in the original Deere theme. This is a clean, self-contained feature that would be excellent for upstreaming.

## Key Differences

### 1. Mixer Column Volume/Gain (`mixer_column_volume_gain.xml`)

**Original Deere (47 lines):**
- Simple vertical layout with gain knob and volume fader
- No mute button
- Gain knob is always centered

**deere-redo (92 lines):**
- Adds horizontal layout wrapper around gain knob
- Includes mute button (`[Channel],mute` control) styled as `EQKillButton`
- Uses expanding spacers to center gain knob when kill buttons are hidden
- Mute button visibility controlled by `[Skin],show_eq_kill_buttons` setting
- Button size: 15f x 20f

### 2. 4-Deck Mixer Controls (`mixer_controls_4decks_left.xml` & `_right.xml`)

**Original Deere:**
- Gain knob pushed to right side with single expanding spacer
- No mute button

**deere-redo:**
- Adds mute button before the gain knob
- Uses conditional spacer that only shows when kill buttons are hidden
- Same mute button styling and behavior as mixer column

### 3. Styling (style.qss)

**Both themes have identical `EQKillButton` styling:**
```css
#EQKillButton {
  border-radius: 2px;
  border: 0px;
  margin: 0px/1px  /* deere-redo uses 0px, original uses 1px */
}

#EQKillButton[value="1"] {
  background-color: #bb0000;
}
#EQKillButton[value="1"]:hover {
  background-color: #e30000;
}
```

**Minor difference:** deere-redo uses `margin: 0px`, original uses `margin: 1px`

### 4. Skin Settings

**Both themes:**
- Already have `[Skin],show_eq_kill_buttons` setting defined
- Setting is exposed in skin settings UI as "Kill Switches"
- Default value is 1 (enabled)

## What Needs to be Upstreamed

### Files to Modify:
1. **`mixer_column_volume_gain.xml`** - Add mute button and layout changes
2. **`mixer_controls_4decks_left.xml`** - Add mute button before gain knob
3. **`mixer_controls_4decks_right.xml`** - Add mute button before gain knob
4. **`style.qss`** (optional) - Change margin from 1px to 0px for consistency

### Files Already Compatible:
- `skin.xml` - Setting already defined
- `skin_settings.xml` - UI toggle already exists
- All EQ kill button styling already present

## Technical Details

### Control Used:
- `[Channel<N>],mute` - Standard Mixxx control for channel muting

### Layout Pattern:
```xml
<WidgetGroup>
  <Layout>horizontal</Layout>
  <Children>
    <!-- Optional: expanding spacer when buttons hidden -->
    <WidgetGroup visible="![Skin],show_eq_kill_buttons">
      <Size>0me,1min</Size>
    </WidgetGroup>
    
    <!-- Mute button -->
    <PushButton visible="[Skin],show_eq_kill_buttons">
      <TooltipId>mute</TooltipId>
      <Size>15f,20f</Size>
      <ObjectName>EQKillButton</ObjectName>
      <Connection>
        <ConfigKey>[Channel<N>],mute</ConfigKey>
      </Connection>
    </PushButton>
    
    <!-- Gain knob -->
    <Template src="skin:knob.xml">
      <SetVariable name="control">pregain</SetVariable>
    </Template>
  </Children>
</WidgetGroup>
```

## Benefits of Upstreaming

1. **User-requested feature** - Provides quick channel muting without touching faders
2. **Consistent with existing UI** - Reuses existing kill button styling and settings
3. **Non-breaking** - Controlled by existing `show_eq_kill_buttons` setting
4. **Clean implementation** - Minimal code changes, follows existing patterns
5. **Works in all layouts** - Properly adapts to 2-deck and 4-deck modes

## Recommendation

This is an **excellent candidate for upstreaming** because:
- It's a discrete, well-contained feature
- Uses existing infrastructure (settings, styling)
- Provides clear user value
- Implementation is clean and follows theme conventions
- No breaking changes or dependencies
