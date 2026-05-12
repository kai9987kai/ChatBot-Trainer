# Advanced Chatbot Research Hybrid v2.5

A single-file browser-based chatbot research lab built with **TensorFlow.js**, **TF-IDF text features**, **neural classification**, **model distillation**, **retrieval fallback**, and configurable **confidence-gated chat modes**.

This project is designed as an experimental local AI chatbot sandbox where you can train a model in the browser, distill it into a smaller model, save/load it locally, import/export datasets, and test different inference strategies without needing a backend server.

---

## ✨ Features

### Core Chatbot System

- Browser-only chatbot powered by TensorFlow.js
- Train a larger neural model directly in the browser
- Distill the large model into a smaller model
- Chat with the active model after training or loading
- Uses TF-IDF vectors with n-gram support
- Lightweight stemming/token normalization
- Conversation history support
- Confidence scoring for every response
- Top-k output inspection

---

## 🧠 Hybrid AI Architecture

The chatbot uses a hybrid inference pipeline:

1. **TF-IDF Feature Extraction**
   - Converts user text into normalized numerical vectors.
   - Supports unigrams and bigrams.
   - Uses inverse document frequency to weight important terms.

2. **Neural Classifier**
   - A TensorFlow.js dense neural network predicts the most likely response class.
   - The model outputs probabilities over known response examples.

3. **Retrieval Fallback**
   - Finds the closest training examples using cosine similarity.
   - Helps the chatbot respond better when the neural model is uncertain.

4. **Hybrid Scoring**
   - Combines neural probability and retrieval similarity.
   - The neural/retrieval balance can be controlled from the UI.

5. **Confidence Gate**
   - When enabled, low-confidence responses are blocked.
   - When disabled, the active model can answer even if confidence is weak.

---

## 🆕 New in v2.5

### Confidence Gate Toggle

A new chat option allows you to turn confidence protection on or off.

- **Confidence Gate ON**
  - The chatbot abstains when confidence is below the threshold.
  - Useful for safer testing and dataset improvement.

- **Confidence Gate OFF**
  - The chatbot always attempts to answer using the current model.
  - Useful for debugging, experimentation, and direct model behavior testing.

---

### Chat Mode Switch

You can now switch between two chat modes:

#### Hybrid Mode

Uses:

- Neural model prediction
- Retrieval fallback
- Hybrid scoring
- Confidence logic

Best for general use.

#### Current Model Mode

Uses:

- Only the active neural model
- No retrieval fallback
- No hybrid scoring

Best for directly testing what the trained or loaded model has learned.

---

## 🧪 Research Lab Features

### Training Dashboard

The app includes:

- Large model training
- Small model distillation
- Training loss chart
- Validation split
- Early stopping
- Stop training button
- Evaluation button
- Tensor cleanup to reduce memory leaks

---

### Dataset Lab

You can:

- Import datasets from JSON
- Export the current dataset
- Add new examples manually
- Clean and deduplicate examples
- Restore the starter dataset
- Import older project formats

Supported dataset formats include:

```json
[
  {
    "input": "hello",
    "output": "Hi there! How can I help you today?"
  }
]
````

Also supported:

```json
{
  "rawData": [
    {
      "input": "what is tfidf",
      "output": "TF-IDF scores word importance across documents."
    }
  ]
}
```

And simple conversational turns:

```json
{
  "conversationalRawData": [
    [
      {
        "speaker": "user",
        "text": "hello"
      },
      {
        "speaker": "bot",
        "text": "Hi there!"
      }
    ]
  ]
}
```

---

## 💾 Save and Load

The chatbot supports both file-based and local browser persistence.

### Download Model

Exports:

* TensorFlow.js model JSON
* Weight `.bin` file
* Metadata JSON file

The metadata stores:

* Vocabulary
* IDF values
* Output labels
* Dataset
* Inference settings
* Confidence settings
* Chat mode settings

---

### IndexedDB Save

You can save the active model locally in the browser using IndexedDB.

This allows you to close the page and reload the model later without retraining.

---

## 🗂 Project Structure

This project is designed as a single-file web app:

```text
index.html
```

Everything is included in one file:

* HTML layout
* CSS styling
* TensorFlow.js model logic
* Dataset handling
* Chat interface
* Save/load tools
* Retrieval system
* Confidence controls

---

## 🚀 Getting Started

### 1. Clone or Download

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Or download the HTML file directly.

---

### 2. Open the App

Because this is a browser app, you can open it directly:

```text
index.html
```

For best results, use a local development server:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

---

## 🧠 How to Use

### Train a Model

1. Open the app.
2. Click **Train Large Model**.
3. Wait for training to finish.
4. Click **Distill to Small Model** if you want a smaller deployable model.
5. Start chatting.

---

### Chat With the Current Model

1. Train or load a model.
2. Use the chat box.
3. Toggle **Chat Mode: Current Model**.
4. Send messages to test the active neural model directly.

This mode is useful when you want to see exactly what the model learned without retrieval fallback helping it.

---

### Use Confidence Gate

Use the **Confidence Gate** button in the chat area.

#### ON

```text
Confidence Gate: ON
```

The chatbot refuses low-confidence predictions.

#### OFF

```text
Confidence Gate: OFF
```

The chatbot answers using the model even if confidence is low.

This is useful for experimental testing.

---

## ⚙️ Main Controls

| Control                     | Purpose                                        |
| --------------------------- | ---------------------------------------------- |
| Train Large Model           | Trains the main neural model                   |
| Distill to Small Model      | Trains a smaller model from the large model    |
| Download Small Model + Meta | Saves model files and metadata                 |
| Save to IndexedDB           | Saves model locally in browser storage         |
| Load from IndexedDB         | Loads saved local model                        |
| Load from JSON + BIN + Meta | Loads downloaded model files                   |
| Export Dataset              | Downloads the training dataset                 |
| Import Dataset JSON         | Adds examples from a JSON file                 |
| Clean + Deduplicate         | Removes duplicate/invalid examples             |
| Add / Teach Example         | Adds a new training pair                       |
| Confidence Gate             | Turns low-confidence blocking on/off           |
| Chat Mode                   | Switches between Hybrid and Current Model chat |

---

## 🧬 Model Design

### Large Model

The large model uses dense layers with:

* ReLU activations
* Dropout
* L2 regularization
* Batch normalization
* Softmax output

It is intended as the stronger teacher model.

---

### Small Model

The small model is a compact student network.

It is trained through distillation by learning from the large model’s soft predictions.

This makes it useful for faster local inference.

---

## 🔍 Retrieval System

The retrieval system compares the user input vector against all training example vectors using cosine similarity.

This helps the chatbot when:

* The neural model is uncertain
* The input closely matches a known training example
* The dataset is small
* The phrasing is similar but not identical

---

## 📊 Confidence System

The app tracks:

* Neural model probability
* Retrieval cosine similarity
* Final confidence score
* Decision path
* Top predicted outputs

Example decision paths:

```text
hybrid
neural-high-confidence
retrieval-exact-ish
low-confidence-abstain
current-model-only
current-model-only-low-confidence-allowed
```

---

## 🧪 Suggested Experiments

Try testing:

* Hybrid mode vs Current Model mode
* Confidence Gate ON vs OFF
* Small model vs large model
* Small dataset vs augmented dataset
* Many short examples vs fewer detailed examples
* Duplicate/conflicting examples
* Higher/lower confidence threshold
* Higher/lower neural weight

---

## 📁 Example Dataset

```json
[
  {
    "input": "hello",
    "output": "Hi there! How can I help you today?"
  },
  {
    "input": "what is machine learning",
    "output": "Machine learning is a type of AI where systems learn patterns from data."
  },
  {
    "input": "how do i improve this chatbot",
    "output": "Add more examples, clean the dataset, use retrieval fallback, and evaluate confidence."
  }
]
```

---

## 🛠 Customization Ideas

You can extend the project with:

* Larger starter datasets
* More advanced tokenization
* Trigram support
* Synonym expansion
* Spell correction
* Sentiment detection
* User profile memory
* Dataset quality scoring
* Synthetic paraphrase generation
* Model cards
* Web Worker training
* WebGPU/WebNN support when available
* Local embedding models
* RAG-style document search
* Voice input/output
* Multi-personality response modes

---

## ⚠️ Limitations

This is not a full large language model.

The chatbot:

* Selects from known output classes
* Does not freely generate new long-form answers
* Depends heavily on dataset quality
* Can be confused by contradictory training examples
* May overfit small datasets
* Cannot know facts that are not in the dataset
* May give incorrect answers if trained on incorrect examples

For best results, use diverse, clean, balanced training examples.

---

## 🔐 Privacy

The app runs locally in the browser.

By default:

* Training happens client-side
* Chat happens client-side
* IndexedDB saves data locally
* No backend server is required

External network access is only used to load TensorFlow.js from the CDN unless you self-host the library.

---

## 📦 Dependencies

Loaded via CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@4.13.0/dist/tf.min.js"></script>
```

No build step is required.

---

## 🧭 Roadmap

Planned or possible future upgrades:

* Prototype-centroid memory
* Uncertainty queue for active learning
* Synthetic paraphrase generator
* Dataset audit panel
* Model card export
* Web Worker training thread
* WebGPU backend detection
* Local embedding model support
* Multiple chatbot personalities
* Conversation memory viewer
* Per-topic dataset folders
* Response safety filters
* Import/export full project snapshots
* Benchmark suite for test prompts

---

## 🤝 Contributing

Ideas, improvements, and experimental features are welcome.

Suggested contribution areas:

* Better dataset tools
* More robust NLP preprocessing
* Improved model architecture
* Faster training
* UI/UX polish
* Accessibility improvements
* Browser storage improvements
* Offline-first deployment

---

## 📄 License

Choose a license that fits your project.

Recommended open-source options:

```text
MIT License
Apache-2.0
GPL-3.0
```

---

## Author

Created by **kai9987kai** as an experimental browser-based AI chatbot research lab.

---

## Summary

**Advanced Chatbot Research Hybrid v2.5** is a local AI chatbot sandbox for experimenting with browser-side machine learning, hybrid retrieval, model distillation, confidence scoring, and direct current-model chat testing.

It is ideal for learning how small NLP systems work, testing custom datasets, and building toward more advanced local AI assistants.

```
```
