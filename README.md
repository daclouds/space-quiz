# Space Quiz

A bilingual English/Korean space exploration quiz with 10 questions, a progress
bar, live scoring, answer explanations, and emoji results. Switch languages
without losing progress, and copy your result to share it.

The interface supports light/dark system themes, visible keyboard focus, and
reduced-motion preferences. Scores of 8 or more unlock a celebration.

## Play

Visit **https://daclouds.github.io/space-quiz/**.

## Run locally

Download or clone this repository and open `index.html` directly in your browser.
No server, package installation, or build step is required.

Choose an answer, then use Next or Finish to advance. Use Tab to navigate controls
and Enter or Space to activate buttons. If copying a result is unavailable in
your browser, the app provides selected text for manual copying.

## Checks and deployment

The `GitHub Pages` workflow validates inline JavaScript syntax on pull requests
to `main`. This is a syntax check, not an end-to-end browser test.

For a browser smoke test, check correct and incorrect answers, scoring,
language switching, Next/Finish, results, copying, and restart. Also check
keyboard operation, light/dark themes, and reduced motion.

On pushes to `main`, the workflow publishes only `index.html` to GitHub Pages
after the check passes. It can also be run manually on `main` from Actions.
The repository's Pages source must be set to **GitHub Actions**.
