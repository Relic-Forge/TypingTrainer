# Typing Trainer

Typing Trainer is a single-file, browser-based typing practice app that studies weak spots and turns them into focused training blocks. It runs as a static site with no build step, backend, or account system.

Live demo: https://relic-forge.github.io/TypingTrainer/

## Features

- Personalized typing blocks based on recent performance patterns.
- Focus Points and Statistics screens for weak keys, bigrams, and progress trends.
- Results screen with speed, accuracy, consistency, and improvement details.
- Settings for live WPM, accuracy visibility, keyboard layout, appearance, and saved data.
- Local-only persistence through browser `localStorage`.

## Run Locally

Open `index.html` directly in a browser, or serve the folder with a static server:

```powershell
python -m http.server 8000
```

Then open http://localhost:8000/.

## GitHub Pages Publishing

This repository is ready to publish from the `main` branch root.

1. Push the latest `main` branch to GitHub.
2. Open the repository on GitHub.
3. Go to Settings -> Pages.
4. Set Source to "Deploy from a branch".
5. Set Branch to `main` and Folder to `/ (root)`.
6. Save and wait for GitHub Pages to publish the site.
7. Visit https://relic-forge.github.io/TypingTrainer/.

## Publishing Checklist

- [x] Static entrypoint is named `index.html`.
- [x] README explains the app, local usage, validation, support, and publishing flow.
- [x] MIT license file is present.
- [x] Page metadata, canonical URL, social metadata, theme color, and favicon are configured.
- [x] `.nojekyll` is present so GitHub Pages serves the static files directly.
- [x] Local browser smoke test is complete from a static server.
- [x] Desktop and mobile settings layouts have been checked locally.
- [x] Settings data wipe has been verified locally.
- [ ] GitHub Pages is set to publish from `main` branch root.
- [ ] Browser smoke test is complete on the published Pages URL.
- [ ] Final commit has been pushed to GitHub.

## Validation

The app has no package manifest or build step. Before publishing, check the inline JavaScript syntax by extracting the script and running `node --check`:

```powershell
$html = Get-Content -Raw index.html
$script = [regex]::Match($html, '<script>([\s\S]*)</script>').Groups[1].Value
Set-Content -Path "$env:TEMP\typing-trainer-inline.js" -Value $script
node --check "$env:TEMP\typing-trainer-inline.js"
```

Also do a browser smoke test from a local static server and confirm the welcome flow, navigation, settings panel, theme toggle, keyboard layout menu, data wipe control, and one typing block still work.

## Data And Privacy

Typing Trainer stores progress and settings in the browser's `localStorage`. Data stays on the device and is not sent to a server by this app. Clearing browser storage or using the settings data wipe will remove saved progress.

## Support

Suggestions, bugs, or anything else can be sent on the GitHub page or sent to darkrelicer on Discord.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
