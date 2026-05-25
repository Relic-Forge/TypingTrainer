# TypingTrainer

TypingTrainer is a single-file static typing practice app. Open
`TypingTrainer.html` in a browser to run it locally.

## Current UI

- Home screen with available training blocks, a recommended block, top status
  bar, and bottom navigation.
- Focus Points and Statistics info screens.
- Results screen with a detailed breakdown.
- Settings screen opened from the gear button.

## Settings

The settings screen is a responsive full-screen overlay with a blurred
background underlay and animated panel entrance. It includes:

- General settings for showing or hiding Live WPM and Accuracy in typing
  blocks.
- Keyboard layout selection with an animated custom dropdown for QWERTY,
  DVORAK, Colemak, and AZERTY.
- Appearance setting for toggling light/dark mode.
- Data control for wiping saved progress.

Settings are persisted in browser `localStorage`.

## Validation

The app has no package manifest or build step. For JavaScript syntax checks,
extract the inline script from `TypingTrainer.html` and run `node --check`.
