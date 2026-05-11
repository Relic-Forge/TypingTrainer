# TypingTrainer — Codex Build Spec

Build a single self-contained HTML file application (no external libraries, no frameworks, no backend). Everything must be inside one file using:
- <style> for CSS
- <script> for JavaScript

The app is a personalized typing trainer with 3 main states:
1. DIAGNOSTIC
2. RESULTS
3. DRILLS (SESSION MODE)

The app must behave like a state machine. Only one state is active at a time.

---

# DATA MODEL

Maintain a global state object:

state = {
  phase: "diagnostic | results | drills",
  keystrokes: [],
  startTime: null,
  diagnosticText: "",
  results: {},
  stats: {
    keySpeed: {},
    bigramSpeed: {},
    errors: {},
    sessions: []
  },
  sessionProgress: 0,
  sessionTarget: 1000
}

---

# 1. DIAGNOSTIC PHASE

UI:
- Show a fixed paragraph on first load: "Welcome to Typing Trainer! Typing Trainer is an intelligent program made to help you identify your weak spots in typing, with courses to help fight against these. Typing Trainer includes a personalized typing diagnostic that analyzes real keystroke behavior instead of just speed, tracking timing, accuracy, and rhythm consistency in real time. It provides a detailed performance breakdown after each session, including weak keys, difficult key pairs, and hesitation patterns that affect flow. The system feeds you adaptive practice drills based on your weaknesses, prioritizing problem areas instead of using generic lessons. Understand that Typing Trainer is to help build accuracy (and some speed). Did you know 40% of people type under a 90% accuracy? Let’s train and improve!"

Rules:
- Do NOT start timing until FIRST keypress
- Record performance.now() on every keypress
- Track:
  - key timing
  - bigram timing
  - errors
  - rhythm spikes (>300ms gaps inside words)

---

# 2. RESULTS PHASE

Triggered when diagnostic is complete.

Show:
- WPM
- Accuracy %
- Top 5 slowest keys
- Top 5 slowest bigrams
- Rhythm instability indicator

Render keyboard visualization:
- green = fast
- yellow = medium
- red = slow

Button:
Start Training Session

---

# 3. DRILLS PHASE

Session rule:
- 1000 CORRECT characters = session complete

UI:
- live WPM
- live accuracy
- progress bar

Drill generation:
- use built-in word list
- weight weak keys and bigrams (~60%)
- must form readable English sentences

---

# STORAGE

Use localStorage only.

Save:
- sessions
- WPM history
- accuracy history
- weak keys
- best WPM

Save after every session.

Include:
- export JSON button
- import JSON button

---

# UI RULES

- Dark theme
- Minimal design
- Highlight current character
- correct = green
- incorrect = red (must be fixed before continuing)
- smooth transitions between phases

---

# ARCHITECTURE RULES

- Single HTML file only
- Offline capable
- No frameworks
- Modular JS functions per phase

---

# CORE RULE

Phases must never mix:
- Diagnostic = measurement only
- Results = read-only summary
- Drills = training loop