# Copilot instructions for `space-quiz`

## Build, test, and lint commands

This repository is intentionally a **single-file static app** (`index.html`) with **no build system**, **no test runner**, and **no linter configuration**.

- **Run locally:** open `index.html` directly in a browser (`file://.../index.html`)
- **Smoke test (manual):** verify quiz flow end-to-end in browser
  - Answer flow: choice selection, correct/incorrect feedback, score updates
  - Explanation flow: each of the 10 questions has English/Korean explanations, shown only after answering; wrong feedback names the correct choice. Switch languages after correct and incorrect answers to verify translated explanations without score/progress changes.
  - Navigation flow: Next/Finish progression through 10 questions
  - Result flow: result screen text/emoji, restart behavior
  - Copy result flow: localized summary, announced clipboard success, selected-text fallback when clipboard access is denied/unavailable, and cleanup on restart. Exercise both English and Korean; simulated key events plus `.click()` do not verify native keyboard activation.
  - Keyboard flow: focus visibility and Enter activation for language toggle, answers, next/restart

## High-level architecture

- The entire app (markup, styling, behavior) is implemented in **one document**: `index.html`.
- UI has two view states:
  - `#quiz-screen` for active questions
  - `#result-screen` for completion summary
  - Visibility is toggled with the shared `.hidden` class.
- Data and localization are centralized in the `i18n` object:
  - `en` and `ko` each provide UI strings plus a full 10-question dataset.
  - `getT()` returns the active locale bundle, and `getQuizData()` reads questions from that bundle.
- State is held in top-level JS variables:
  - `currentLang`, `currentIndex`, `score`, `userAnswers`
  - Rendering is driven by `renderQuestion()`, `showResults()`, and `update*` helpers.
- Interaction model:
  - Answers are generated as `.choice-btn` buttons per question render.
  - `handleAnswer()` records selection and updates score.
  - `applyAnsweredState()` handles feedback and answer lock state.
  - `nextStep()` advances questions and transitions to results.
  - `restartQuiz()` resets all session state.

## Key conventions in this codebase

- **No external dependencies:** keep everything self-contained in `index.html` unless explicitly changing project scope.
- **Language switching is runtime, not page reload:** use `switchLanguage()` + `updateStaticTexts()` and preserve current quiz progress/state.
- **Question content is locale-owned:** each language bundle includes its own `questions` array; when adding/editing questions, keep `en` and `ko` structures aligned (same count/order/answer indices).
- **Answer explanations:** each question owns an `explanation`; `applyAnsweredState()` renders it with text nodes in the atomic `#feedback` status region. Keep explanations in the theme's readable text color, rather than relying on correct/incorrect colors. Actual answer submission focuses Next; advancing focuses the new first choice, while language re-rendering must not steal focus.
- **Theme consistency via CSS variables:** light/dark behavior is controlled through `:root` + `@media (prefers-color-scheme: dark)` tokens (`--bg`, `--surface`, `--text`, etc.).
- **Motion/accessibility expectations are first-class:** respect `prefers-reduced-motion` rules already present and maintain visible `:focus-visible` styling for all interactive controls.
- **Result celebration contract:** `showResults()` applies `.celebrate` only when `score >= 8`; keep this threshold behavior intact unless requirements change.
- **Result sharing:** `copyResult()` uses the Clipboard API with a visible manual-copy fallback. `copyRequest` invalidates pending clipboard completions when results are refreshed or the quiz restarts; preserve this guard.
