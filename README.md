# Healthcare NER with CRF (NLP Assignment)

A compact pipeline to **identify diseases and treatments** in healthcare text using:

- **spaCy** for tokenization & PoS tags
- **Custom feature engineering** (word shape, suffixes, capitalization, PoS, context)
- **CRF (sklearn-crfsuite)** for sequence labeling (labels: `D` for Disease, `T` for Treatment, `O` for Other)

---

## ✨ What the project does
- Reconstructs sentences and labels from token-per-line files
- Builds feature vectors per token (current + previous token features)
- Trains a **CRF** model (c1=1.0, c2=0.01, max_iter=100)
- Evaluates on a test set (F1/Precision/Recall/Accuracy)
- Extracts **Disease → Treatment** pairs from CRF predictions

---

## 📁 Data format
- Input files contain **one word per line**; sentences are separated by **blank lines**.
- Example inputs referenced in code:
  - `data.txt`, `train_label`, `test_sent`, `test_label`

---

## 🧠 Key components
- `extract_sentences_from_file(path)`: rebuilds sentences from word-per-line files
- `extract_word_features(sentence, pos, pos_tags)`: token features
- `extract_sentence_words_features(sentence)`: features list per sentence
- `extract_sentence_words_labels(labels)`: labels list per sentence
- `CRF` training & prediction (`sklearn_crfsuite`)
- Post-processing to build **Disease→Treatment dictionary**

---

## 📊 Results (from the script)
- **F1-score** (weighted): **0.9145**
- **Precision** (weighted): **0.9141**
- **Recall / Accuracy**: **0.9205**

> The code also shows example mappings, e.g. `hereditary retinoblastoma → radiotherapy`.

---

## 🛠 Requirements
- Python ≥ 3.8
- `spaCy`, `sklearn-crfsuite`, `pandas`

Install:
```bash
pip install spacy sklearn-crfsuite pandas
python -m spacy download en_core_web_sm
```

---

## ▶️ How to run
1. Place input files (`data.txt`, `train_label`, `test_sent`, `test_label`) in the working directory.
2. Run the script:
   - Preprocess data → build features
   - Train **CRF**
   - Evaluate & print metrics
   - Generate **Disease→Treatment** pairs
3. Query examples (as in the script):
   ```python
   disease = 'hereditary retinoblastoma'
   print(disease_treatment_dict[disease])  # radiotherapy
   ```

---

## ⚠️ Notes
- CRF hyperparameters (`c1=1.0`, `c2=0.01`) were selected by simple experimentation.
- Features use **current** and **previous** token context; you can extend to next-token features.
- This is a **proof-of-concept**; accuracy depends on data quality and label consistency.

---

## 📄 License
Educational use.
