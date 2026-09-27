# Typing Trainer

Typing Trainer is a single-file, browser-based typing practice app that studies weak spots and turns them into focused training blocks. It runs as a static site with no build step, backend, or account system.

Try it here: https://relic-forge.github.io/TypingTrainer/

## Features

- A short placement test, then personalized practice blocks built from your slowest keys and letter pairs.
- Drills made of real, hand-written sentences chosen to cover the keys and pairs you need to work on.
- Results screen with a score, a keyboard speed heatmap, comparison with your recent average, and one-key next steps (next block, retry, or home).
- Focus Points and Statistics screens for weak keys, bigrams, and progress trends.
- A daily streak that counts both the placement test and practice blocks.
- Keyboard-friendly throughout: arrow keys switch tabs, `,` opens Settings, and the full shortcut list is in Settings.
- Light and dark mode, plus settings for live WPM, live accuracy, keyboard layout (QWERTY, Dvorak, Colemak, AZERTY), and saved data.
- Local-only persistence through browser `localStorage`.

## Run Locally

Open `index.html` directly in a browser.

## Data And Privacy

Typing Trainer stores progress and settings in the browser's `localStorage`. Data stays on the device and is not sent to a server by this app. Clearing browser storage or using the settings data wipe will remove saved progress.

## Support

Suggestions, bugs, or anything else can be sent on the GitHub.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
