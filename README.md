# 😄 Emotion Prediction

A deep learning web app that detects the **emotion behind a piece of text**. A **Bidirectional GRU (BiGRU)** model built with TensorFlow/Keras classifies a sentence into one of six emotions, and a **FastAPI** backend serves both the prediction API and a simple web UI.

---

## ✨ Features

- Classifies text into 6 emotions: 😢 sadness, 😄 joy, ❤️ love, 😠 anger, 😨 fear, 😲 surprise
- Returns the top emotion, its confidence, and the **full probability breakdown** for all emotions
- BiGRU model trained in a Jupyter notebook (`final_clean.ipynb`)
- Model and tokenizer are loaded **once at server startup**, so predictions stay fast
- Built-in web UI served at `/` and a health-check endpoint at `/health`
- Input validation with Pydantic (1–2000 characters) and CORS enabled

---

## 📂 Project Structure

```
Emotion-Prediction/
├── main.py              # FastAPI app (model loading, preprocessing, API endpoints)
├── final_clean.ipynb    # Data cleaning, preprocessing, and BiGRU model training
├── Artifacts/
│   ├── BiGRU_Model.keras   # Trained BiGRU model
│   └── tokenizer.pkl       # Fitted tokenizer used at training time
├── static/              # Frontend (served at "/")
├── requirements.txt     # Python dependencies (pinned versions)
├── runtime.txt          # Python runtime specification for deployment
└── README.md
```

---

## 🛠️ Tech Stack

| Layer      | Tools                                              |
|------------|----------------------------------------------------|
| Deep Learning | Python, TensorFlow / Keras (BiGRU), NumPy       |
| Backend    | FastAPI, Uvicorn, Pydantic                         |
| Frontend   | HTML / CSS / JavaScript (in `static/`)             |
| Notebook   | Jupyter                                            |

---

## 🧠 How It Works

When you send a sentence to `/predict`, the server runs it through this pipeline:

1. **Clean the text**: lowercase, remove apostrophes (`can't` → `cant`), strip special characters and punctuation, collapse extra spaces. This matches the preprocessing used during training.
2. **Tokenize**: convert words to integer sequences with the saved tokenizer.
3. **Pad**: pad or truncate to a fixed length of **50** tokens.
4. **Predict**: the BiGRU model outputs a probability for each of the 6 emotions.
5. **Respond**: return the highest-probability emotion with its confidence and all probabilities.

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/CodaptorShafa/Emotion-Prediction.git
cd Emotion-Prediction
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

> Check `runtime.txt` for the Python version this project targets. TensorFlow 2.17 requires a compatible Python version.

### 4. Run the server

```bash
uvicorn main:app --reload
```

- Web UI: `http://127.0.0.1:8000/`
- Interactive API docs (Swagger): `http://127.0.0.1:8000/docs`

---

## 🔌 API Reference

### `GET /`
Serves the web UI.

### `GET /health`
Checks that the server is up and the model is loaded.

```json
{
  "status": "Server is running",
  "model_loaded": true
}
```

### `POST /predict`

**Request body**

```json
{
  "text": "I feel so happy and excited"
}
```

**Response** (probability values are illustrative)

```json
{
  "text": "I feel so happy and excited",
  "predicted_emotion": "joy",
  "confidence": 0.93,
  "all_probabilites": {
    "sadness": 0.01,
    "joy": 0.93,
    "love": 0.02,
    "anger": 0.01,
    "fear": 0.01,
    "surprise": 0.02
  }
}
```

**cURL example**

```bash
curl -X POST "http://127.0.0.1:8000/predict" \
  -H "Content-Type: application/json" \
  -d '{"text": "I feel so happy and excited"}'
```

If the model has not finished loading, the API returns `503 Service Unavailable`.

---

## 🧪 Model Training

The full workflow, from data cleaning and text preprocessing to tokenization and training the BiGRU model, lives in [`final_clean.ipynb`](final_clean.ipynb). The trained model and tokenizer are saved into the `Artifacts/` folder and loaded by `main.py` at startup.

---

## ⚠️ Limitations

- The model predicts one of only six emotions and works best on short, English sentences.
- Sarcasm, mixed emotions, and slang may be misclassified.
- Intended for learning and demonstration, not for clinical or high-stakes decisions.

---

## 🔮 Future Improvements

- Add model evaluation metrics (accuracy, F1, confusion matrix) to the README
- Compare against transformer models such as DistilBERT
- Add Docker support and a live deployment link
- Add unit tests for the preprocessing and API layers

---

## 👤 Author

**Shafa** — [@CodaptorShafa](https://github.com/CodaptorShafa)
