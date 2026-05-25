# Typing Trainer Statistics Dashboard Redesign

Read the current repository first. The app is a single-file static app centered around `TypingTrainer.html`. Do not introduce React, Vite, npm, a build step, or external chart libraries. Keep the app runnable by opening `TypingTrainer.html` directly in a browser.

## Goal

Redesign the Statistics page so it is easier to read, smoother to use, and useful whether the user has 0 sessions, 1 session, 10 sessions, or several months of data.

The Statistics page should be visually simple:

- KPI cards
- Graph card
- Graph view toggles

Do not add a bottom/right detail area. Do not add explanatory dashboard copy. Do not add headers like “Your typing trend” or subtitle descriptions. The page should speak for itself.

## Current structure to preserve

The existing app already has:
- A Statistics screen
- Stats tabs: Recent, Daily, Monthly, Top Speeds
- KPI/stat values
- An SVG graph area
- Light/dark theme variables
- LocalStorage-backed session history

Preserve:
- Existing navigation
- Existing settings
- Existing localStorage state
- Existing session history format
- Existing `runtime.statsGraphView`
- Existing result/session recording
- Existing sample/debug controls unless replacing them with better testing controls

## Layout requirements

Replace the current left-side “Other stats” layout with a focused dashboard layout.

Required layout:

1. KPI row at the top of the Statistics content area
   - Average WPM
   - Best WPM
   - Average Accuracy
   - Current Streak
   - Sessions or Blocks Completed

2. Main graph card below the KPI row
   - Large chart area
   - Segmented view toggles at the top of the card
   - Simple legend only when it helps the active graph
   - No extra descriptive headline/subtitle
   - No bottom/right insight area

3. Bottom app navigation remains unchanged.

The graph must be the hero of the page. Do not let labels, legends, or stat text crowd it.

## View behavior

### Recent

Recent must show the last 10 valid sessions.

Do not show 20 sessions in Recent. Do not show all sessions. Show the last 10 only.

Recent should show:
- WPM trend
- Accuracy trend
- Clean left/right axis labels
- Clear x-axis labels
- Latest point highlighted
- Good behavior with 1 session, 2 sessions, and 10 sessions

### Daily

Daily should show practice consistency:
- Rounded bars = completed sessions per day
- Line = average WPM for days with sessions
- Empty days still show as soft background columns
- Axis labels must be clean and readable
- Session count axis must use integer ticks only

### Monthly

Monthly should show longer-term activity:
- Rounded bars = sessions per month
- Line = average WPM per month
- Clean readable month labels
- Do not crowd the x-axis

### Top Speeds

Top Speeds should be a leaderboard, not a normal graph:
- Rank
- WPM
- Accuracy
- Date
- Horizontal bar
- Do not show an accuracy legend unless accuracy is visually represented as a separate mark

## Dynamic axis scaling

Graph axes must dynamically scale to the data so lines stay readable.

Do not lock accuracy to 0–100 if the data is clustered near 98%. For example:
- If accuracy values are 97.5–99.2%, the axis should show a range such as 95–100 or 97–100.
- If accuracy values are 88–99%, use a wider range that still fits the data.
- Accuracy must never exaggerate tiny changes too aggressively, but it also must not flatten a 98% trend into a straight line.

Do not lock WPM to 0–100 if the user is typing around 60 WPM. For example:
- If WPM values are 58–64, the axis should show a range such as 56–66.
- If WPM values are 35–72, use a wider range that fits the data.
- Always include enough visual padding above and below the data.

Implement helpers:
- `clampNumber(value, min, max)`
- `niceNumber(value)`
- `niceTicks(min, max, targetCount)`
- `getPaddedDomain(values, options)`
- `getWpmDomain(values)`
- `getAccuracyDomain(values)`
- `thinLabels(items, maxLabels)`

Axis rules:
- Axis labels must not repeat.
- Axis labels must not overlap.
- Session count ticks must be integers.
- WPM ticks should be whole numbers.
- Accuracy ticks should include `%`.
- A single data point must not render as a broken line.

## Data-state rules

### 0 sessions

Show a clean empty state inside the graph card:
- No fake chart
- Minimal text: “Complete a typing block to start building your graph.”

### 1 session

Do not draw a useless flat line.

Show a single-session graph state:
- One clear point
- WPM value
- Accuracy value
- Date or session number
- The point should sit in a sensible dynamic domain, not on the border

### 2–10 sessions

Recent graph:
- Render the last 10 or fewer valid sessions
- Use clean dots and lines
- Every point can be shown
- X labels can be `#1` through `#10` or short dates if readable
- Highlight latest session

### More than 10 sessions

Recent still shows only the last 10.

Daily and Monthly can use bucketing.

## Fluid graph animation requirement

The Statistics screen must feel smooth, responsive, and polished as data changes.

Do not make chart updates feel like instant SVG swaps. Animate the user-facing changes cleanly.

Required animation behavior:
- Tab changes should crossfade or gently slide between chart views.
- Bars should animate upward from the baseline.
- Trend lines should draw or morph smoothly.
- Dots should fade/scale into place with a slight stagger.
- KPI numbers should count up/down when values change.
- Latest session point should animate in when new data is added.
- Axis labels and grid lines should transition softly instead of popping.
- Empty state → single session → multi-session should feel intentional and smooth.
- Top Speeds leaderboard rows should slide/fade into place.
- Hover/focus states should be subtle and responsive.

Technical requirements:
- Keep everything single-file and dependency-free.
- Use native SVG, CSS transitions, CSS keyframes, and `requestAnimationFrame` where needed.
- Use stable data keys for graph elements:
  - session date / session index for Recent
  - date bucket key for Daily
  - month key for Monthly
  - session id/date/index for Top Speeds
- Do not add Chart.js, D3, React, Vite, npm, or a build step.
- Respect `prefers-reduced-motion: reduce`.
- In reduced-motion mode, use very short fades or no animation.
- Target timing:
  - 180–240ms for UI transitions
  - 280–420ms for graph transitions
  - 500–700ms max for initial chart draw
- Use clean easing such as `cubic-bezier(0.22, 0.8, 0.22, 1)`.
- No bouncy/cartoon easing for graphs.

## Refactor target

Refactor the Statistics code into clearer functions.

Suggested functions:
- `validStatSessions()`
- `getStatsSummary(sessions)`
- `renderStatsKpis(summary)`
- `animateNumber(el, from, to, formatter)`
- `renderStatisticsPage()`
- `renderStatsChart(view, sessions)`
- `renderEmptyStatsState()`
- `renderSingleSessionState(session)`
- `renderRecentTrendSvg(sessions, dims)`
- `renderActivitySvg(sessions, dims, mode)`
- `renderTopSpeedsList(sessions)`
- `getWpmDomain(values)`
- `getAccuracyDomain(values)`
- `niceTicks(min, max, targetCount)`
- `renderAxisLabels(axisConfig)`

Avoid one giant function that does everything.

## CSS direction

Add or replace classes around the Statistics page:

- `.stats-dashboard`
- `.stats-kpi-grid`
- `.stats-kpi-card`
- `.stats-kpi-label`
- `.stats-kpi-value`
- `.stats-chart-card`
- `.stats-chart-toolbar`
- `.stats-view-tabs`
- `.stats-chart-stage`
- `.stats-graph-svg`
- `.stats-graph-axis`
- `.stats-graph-grid`
- `.stats-graph-label`
- `.stats-graph-line`
- `.stats-graph-dot`
- `.stats-empty-state`
- `.stats-leaderboard`

Visual direction:
- Dark mode should look polished and high contrast.
- Light mode must still work.
- KPI cards should be readable but not oversized.
- Graph card should own most of the space.
- Graph should not require scrolling on a normal fullscreen desktop/laptop view.
- Mobile can wrap, but desktop/fullscreen should be clean.

## Colors

Use existing theme variables where possible.

Recommended visual roles:
- WPM: green
- Accuracy / avg WPM line: blue
- Grid: muted gray
- Axis: readable muted white/gray
- Latest point: subtle white/accent ring

Do not rely on color alone. Labels and legends must make the chart understandable.

## Prohibited content

Do not include:
- “Stable” next to accuracy
- “Your typing trend”
- Long explanatory chart descriptions
- Bottom/right detail panels
- Extra prose-heavy dashboard sections
- A graph library
- A build system

## Testing

Add or improve local sample states so these can be tested quickly:

- 0 sessions
- 1 session
- 2 sessions
- 10 recent sessions
- 14 daily sessions
- 90 days with mixed activity
- 10+ top speeds

Validation steps:
1. Open `TypingTrainer.html` locally.
2. Test light mode.
3. Test dark mode.
4. Test Recent, Daily, Monthly, Top Speeds.
5. Confirm Recent shows last 10 only.
6. Confirm axes dynamically scale.
7. Confirm axis labels are readable and do not repeat.
8. Confirm graph animation is smooth.
9. Confirm reduced-motion mode is respected.
10. Extract the inline script and run `node --check`.

## Acceptance criteria

- Statistics page contains only KPI cards, graph, graph toggles, and required navigation.
- Recent shows last 10 sessions.
- Axis labels are clean and readable.
- WPM and Accuracy dynamically scale to the available data.
- One-session and empty states look intentional.
- Graph transitions are smooth and polished.
- No external dependencies or build step are added.
- Existing app flow still works.
