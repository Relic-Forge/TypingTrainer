# TypingTrainer UI + Navigation Implementation Prompt

You are working in the `Relic-Forge/TypingTrainer` repository.

Do not assume the app structure. Read the existing code first, then implement the requested outcome using the current architecture.

Before changing anything:
- Inspect the full file tree.
- Identify the framework and entry points.
- Find the main app component, routing/navigation logic, typing block selection flow, typing session screen, and results/completion screen.
- Find where styling is currently handled.
- Briefly summarize the relevant files before editing.

Desired outcome:
Update the typing trainer UI and navigation so it matches the homepage direction shown in the screenshot and behaves correctly after any typing block is completed.

## Core requirements

### 1. Home screen
- The app should land on a Home screen showing “Available Blocks.”
- Top bar:
  - Dark header.
  - Settings gear/icon on the left.
  - Centered title: “Available Blocks”.
  - Time and score/streak info on the right.
- Main area:
  - Show available typing blocks.
  - Include a recommended block treatment.
  - Keep the layout more freeform and playful, not a rigid grid of identical rounded rectangles.
- Bottom navigation:
  - Home
  - Focus Points
  - Statistics
- Home should visually follow the provided screenshot, but improve it where the current code/design system allows.

### 2. Results screen behavior
- Every result/completion screen for every typing block must include one clear button that returns the user to the Home screen.
- This must work for any existing block and future blocks that reuse the same results flow.
- Button label: “Back to Home”.
- Do not add multiple competing Home/back buttons.
- Do not route the user to the block list, settings, or previous screen by accident. It must return to the actual Home screen.

### 3. No fullscreen scrolling
- When the app is fullscreened, the user should not need to scroll to see core content.
- Prevent document/body/page scrolling.
- Use a fixed-height app shell based on the viewport.
- Main screen content must fit inside the visible app area between top bar and bottom nav.
- Avoid layout choices that push content below the fold.
- Only use internal scrolling if absolutely necessary, and not on the main Home/results experience.

### 4. Secondary screens
- Focus Points and Statistics currently remain simple placeholder screens.
- Settings is now a designed full-screen overlay opened from the gear icon in
  the top-left.
- Settings should remain responsive across desktop, short-height desktop, and
  mobile viewports.
- The settings overlay should keep the previous screen visible as a blurred
  underlay while the settings panel animates in.
- Do not reintroduce page-level scrolling or edge gaps around the settings
  overlay during transitions.

### 5. Preserve existing trainer functionality
- Do not break typing block selection.
- Do not break typing sessions.
- Do not break scoring, timing, accuracy, or results.
- Keep the implementation scoped to layout, navigation, placeholder screens, and the result-screen Home return path.

## Implementation expectations

- Use the existing framework and coding style.
- Modify the smallest practical set of files.
- Reuse existing components/state where sensible.
- If navigation is currently ad hoc, create a simple centralized screen state or routing pattern.
- If there is already routing, use it properly instead of adding parallel navigation logic.
- Keep component names clear, such as `HomeScreen`, `ResultsScreen`, `FocusPointsScreen`, `StatisticsScreen`, and `SettingsScreen` only if they fit the existing code style.
- Keep CSS/layout changes maintainable.
- Avoid hardcoded hacks that only work for one screen size.

## Acceptance checks

- App loads directly to Home.
- User can select a typing block from Home.
- Typing session still works.
- Completing any block shows a results/completion screen.
- Results screen has exactly one “Back to Home” button.
- Clicking “Back to Home” returns to the Home screen from the screenshot direction.
- Bottom nav switches between Home, Focus Points, and Statistics.
- Gear icon opens the Settings overlay.
- Settings provides General, Appearance, and Data sections.
- Live WPM and Accuracy toggles affect the typing block metric cards.
- Keyboard layout uses the custom animated dropdown.
- Dark mode toggles the app theme and persists in localStorage.
- Fullscreen view does not require page scrolling.
- No regression to scoring, block selection, or typing behavior.

## After implementation

- List the files changed.
- Explain how the app navigation now works.
- Explain how the results screen returns to Home.
- Note any assumptions made from the current codebase.
- Run the available build/test/lint command and report the result.
