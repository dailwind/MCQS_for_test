# AI Studio prompt: update the existing MedMCQ app (do not rebuild)

My Android MCQ app is already built. Do NOT rebuild it or change the UI design. Only make it load MCQs from my GitHub repo and keep them updated.

Data source (public, raw GitHub):
- Manifest: https://raw.githubusercontent.com/dailwind/MCQS_for_test/main/manifest.json
- Full file: https://raw.githubusercontent.com/dailwind/MCQS_for_test/main/medical_mcqs.json
- Per subject: https://raw.githubusercontent.com/dailwind/MCQS_for_test/main/subjects/<slug>.json (file names are listed in the manifest)

manifest.json fields: schemaVersion, dataVersion (like "2026-11-10-2"), totalQuestions, sha256 (of medical_mcqs.json), fullFile, subjects[] = {subject, file, count, topics}.

Each question: {id, book, topic, question, options[4], answerIndex (0-3), explanation}. "book" is the subject name.

What to change:
1. On app start (and on pull-to-refresh / a "Check for updates" button in Settings), download manifest.json. Add "?t=<timestamp>" to the URL to avoid cache.
2. If manifest.dataVersion is different from the saved version, download medical_mcqs.json, check its SHA-256 equals manifest.sha256, then replace all questions in the Room database inside ONE transaction. Save the new dataVersion.
3. If there is no internet or the download/hash fails, keep the current data and do not crash. Keep the bundled JSON in assets as first-run fallback.
4. Keep user progress (answered, correct, bookmarks, stats) keyed by a hash of the question text, not by id, so progress survives updates.
5. Build the subject list dynamically from the data (or manifest.subjects). Do not hard-code subjects or counts. Unknown subjects get a default icon and colour.
6. Show total question count and "Data version" on the home or settings screen. Show a small snackbar like "Updated: 6,954 MCQs" after a successful update.
7. Make sure AndroidManifest.xml has the INTERNET permission. Do downloads on Dispatchers.IO with OkHttp or HttpURLConnection, 30 s timeout. The file is a few MB, so stream-parse it.

Return only the files you changed, with full contents, and a short list of what changed.
