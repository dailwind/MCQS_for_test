# AI Studio Prompt — load MCQs from GitHub

Repo: https://github.com/dailwind/MCQS_for_test (verified: 5,883 questions, 16 subject files). Neeche wala prompt AI Studio mein paste karo aur `medmcq_ui.html` attach kar do. Kuch badalne ki zaroorat nahi.

---

You are a senior Android engineer. Build (or update) my offline-first Android app **MedMCQ** in **Kotlin + Jetpack Compose (Material 3)**, MVVM, Room, Retrofit/OkHttp (or Ktor) and kotlinx.serialization. The attached `medmcq_ui.html` is the exact UI design reference: match its screens, colours, Poppins font, rounded cards, orb mascot with pulse line, floating bottom dock and chat-style quiz.

## Data source: GitHub

Repository: https://github.com/dailwind/MCQS_for_test (branch `main`).

The question bank is hosted on GitHub:

- Manifest: `https://raw.githubusercontent.com/dailwind/MCQS_for_test/main/manifest.json`
- Full file: `https://raw.githubusercontent.com/dailwind/MCQS_for_test/main/medical_mcqs.json`
- Per-subject files: listed in `manifest.subjects[].file` (relative paths, e.g. `subjects/anatomy.json`)

`manifest.json` looks like:

```json
{
  "app": "MedMCQ",
  "schemaVersion": 1,
  "dataVersion": "2026-11-09-1",
  "totalQuestions": 5883,
  "sha256": "...",
  "fullFile": "medical_mcqs.json",
  "subjects": [
    {"subject": "Anatomy", "file": "subjects/anatomy.json", "count": 702, "topics": ["..."]}
  ]
}
```

Each question: `id`, `book`, `topic`, `question`, `options` (4 strings), `answerIndex` (0-3), `explanation`.

## Required behaviour

1. **Bundled fallback**: also ship a copy of `medical_mcqs.json` in `app/src/main/assets/` so the app works on first launch with no internet.
2. **Sync on launch** (background, never blocks the UI):
   - Download `manifest.json`. If `dataVersion` differs from the stored one, download `medical_mcqs.json` (or the subject files), verify the SHA-256, then replace the questions in Room inside one transaction.
   - If download or verification fails, keep the existing data silently.
   - Show a small "Questions updated: X new" snackbar after a successful update.
3. **Keep user progress across updates**: attempts, bookmarks, mistakes and spaced-repetition state must be keyed by a stable hash of the question text (not the `id`), so they survive re-imports.
4. **Settings**: "Check for new questions" button, last-synced time, current data version and total question count.
5. Add `INTERNET` and `ACCESS_NETWORK_STATE` permissions; use HTTPS only; set sensible timeouts and retry with backoff.
6. Subjects list is built dynamically from the data (sorted by question count), so new subjects appear automatically with a default icon.
7. Everything else as in the design: onboarding, home, subjects/topics, practice & exam modes, results, saved (bookmarks/mistakes), progress stats, search, light/dark theme.

## Output

1. Project structure.
2. Every file in full with its path (Gradle files, manifest, data layer with network + Room + sync worker, UI).
3. Steps to build a debug APK and a signed release AAB.

Do not leave TODOs or placeholder code. Everything must compile.
