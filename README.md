# MedMCQ Data

Question bank for the **MedMCQ** app (Dailwind Studios).

## Files

| File | What it is |
|------|------------|
| `manifest.json` | Version, total count, SHA-256 of the full file, and the list of subject files. The app reads this first. |
| `medical_mcqs.json` | All questions in one file. |
| `subjects/*.json` | The same questions split by subject (smaller downloads). |
| `medmcq_ui.html` | The UI design reference / web preview. |

## Question schema

```json
{
  "id": 1,
  "book": "Anatomy",
  "topic": "Upper Limb",
  "question": "…",
  "options": ["A", "B", "C", "D"],
  "answerIndex": 0,
  "explanation": "…"
}
```

- `options` always has 4 items, `answerIndex` is 0–3.
- `id` is unique inside one data version. When new batches are added, ids can change, so the app should match saved progress by question text.

## Raw URLs (after you push)

Replace `USERNAME` and `REPO`:

```
https://raw.githubusercontent.com/USERNAME/REPO/main/manifest.json
https://raw.githubusercontent.com/USERNAME/REPO/main/medical_mcqs.json
https://raw.githubusercontent.com/USERNAME/REPO/main/subjects/anatomy.json
```

## Adding more questions

1. Replace `medical_mcqs.json` and the `subjects/` files with the new export.
2. Update `manifest.json` (new `dataVersion`, `totalQuestions`, `sha256`).
3. Commit and push. The app sees the new `dataVersion` and downloads the update.

## Note

The questions are original practice MCQs and have not been checked against textbooks. Have them reviewed by a qualified clinician before you publish.
