# Lexicon NLP Chatbot — Backend Setup

This connects the `nlp_chatbot.html` UI to a real **NLTK / VADER** engine
instead of the built-in JavaScript approximation.

## 1. Install dependencies

```bash
cd nlp_backend
pip install -r requirements.txt
```

(First run downloads NLTK's `punkt`, `averaged_perceptron_tagger`, and
`vader_lexicon` data automatically — needs internet access once.)

## 2. Run the server

```bash
python backend.py
```

You should see:

```
Lexicon NLP backend running at http://127.0.0.1:5000
```

Leave this running in a terminal.

## 3. Open the chatbot

Open `nlp_chatbot.html` in your browser (double-click it, or use a local
file server). Within a few seconds the header status pill will switch from
**"offline lexicon mode"** to **"NLTK backend connected"**.

From then on:
- **Sentiment Demo** uses real VADER polarity scores (`compound`, `pos`,
  `neu`, `neg`).
- Asking about **POS tagging** and offering a sentence tags it with NLTK's
  `averaged_perceptron_tagger`.
- Asking about **tokenization** and offering a sentence runs NLTK's `punkt`
  tokenizer for real word/sentence splitting.

If the server isn't running (or the browser blocks local requests), the UI
automatically falls back to its offline JavaScript estimators — nothing
breaks, it just says so in a small tag under each result.

## Endpoints

| Method | Path             | Body                | Returns                          |
|--------|------------------|----------------------|-----------------------------------|
| GET    | `/api/health`    | —                    | `{ "status": "ok" }`              |
| POST   | `/api/sentiment` | `{ "text": "..." }`  | VADER polarity scores + label     |
| POST   | `/api/pos`       | `{ "text": "..." }`  | word/tag pairs                    |
| POST   | `/api/tokenize`  | `{ "text": "..." }`  | word tokens + sentence tokens     |

## Notes

- CORS is enabled (`flask-cors`) so the HTML file can call the API even
  when opened directly from disk (`file://`).
- The server binds to `127.0.0.1` only — it isn't exposed to your network.
- To change the port, edit the last line of `backend.py` and the
  `BACKEND_URL` constant near the top of the `<script>` block in
  `nlp_chatbot.html`.
