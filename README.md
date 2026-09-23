# GLS109-03 Water Sustainability – Practice Exam Quizbot (Fall 2026)

Streamlit app. Students pick Exam 1, 2, 3, or the Final. Each run draws 10 short-answer questions (10 pts each), balanced across topics. Gemini grades each answer against the model answer and key points in `question_bank.json`. Nothing is stored: no login, no names, no files, no database.

## Files
- `app.py` – the app
- `question_bank.json` – all questions, model answers, key points (edit this to change content)
- `requirements.txt` – streamlit, google-genai
- `.streamlit/config.toml` – turns off Streamlit usage stats
- `.streamlit/secrets.toml.example` – template for the Gemini key
- `.gitignore` – keeps `secrets.toml` and the `gls109_QUIZZES` folder out of GitHub

## Run locally
1. `pip install -r requirements.txt`
2. Copy `.streamlit/secrets.toml.example` to `.streamlit/secrets.toml` and paste your Gemini API key.
3. `streamlit run app.py`

Without a key the app still works in self-check mode (model answer shown, student scores themselves).

## Deploy (Streamlit Community Cloud)
1. Create a GitHub repo (set it **Private** so students cannot read the answer key in `question_bank.json`).
2. Upload `app.py`, `question_bank.json`, `requirements.txt`, `.streamlit/config.toml`, `.gitignore`, `README.md`. Do **not** upload `secrets.toml` or the past-exam folder.
3. share.streamlit.io → Create app → pick the repo, branch, `app.py`.
4. Advanced settings → Secrets → paste:
   ```
   GEMINI_API_KEY = "your-key"
   ```
5. Deploy. Post the resulting URL on Canvas. Under app Settings → Sharing, make it public so students don't need to log in.

## Editing questions
Each entry in `question_bank.json`:
```json
{"id": "E1-30", "exam": 1, "topic": "Watersheds", "source": "New",
 "question": "...", "model_answer": "...", "key_points": ["...", "..."]}
```
- `exam` = 1, 2, or 3. The Final automatically draws 3 from each exam + 1 extra.
- `topic` controls topic balancing; reuse existing topic names.
- `source` is for you only (not shown to students).
- To drop a question, delete its entry. Validate the JSON (e.g., jsonlint.com) after editing.

## Model
Default grading model: `gemini-3.8-flash`, fallback `gemini-3.5-flash-lite`. Override with `GEMINI_MODEL` in secrets.
