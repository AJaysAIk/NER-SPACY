# NER-SPACY

# Named Entity Recognition with spaCy & CoNLL-2003

Welcome to the ultimate playground for Named Entity Recognition (NER)! This project trains a custom NER model using spaCy and the CoNLL-2003 dataset from Hugging Face. We’ll preprocess the dataset, train the model on the fly, save it to disk, and then evaluate it with some sample predictions. Whether you're a curious coder or a future NLP ninja, this project is your ticket to mastering entity recognition.

## Prerequisites

Make sure you have the following installed on your system:
- Python 3.7+
- pip (for installing Python packages)

> Note: This project also includes optional packages like FastAPI, Uvicorn, and pyngrok if you ever plan to deploy your model as a web service. For the NER training and evaluation, they're not required.

 Installation

1. Clone the Repository

   ```bash
   git clone https://github.com/AJaysAIk/NER-SPACY.git
   cd your-ner-project
Create & Activate a Virtual Environment

Windows:
bash
Copy
python -m venv venv
venv\Scripts\activate
macOS/Linux:
bash
Copy
python3 -m venv venv
source venv/bin/activate
Install Required Packages

You can install the necessary packages using pip:

bash
Copy
pip install spacy datasets fastapi uvicorn pyngrok
Alternatively, if a requirements.txt file is provided:

bash
Copy
pip install -r requirements.txt
Project Structure
 Contains the code to load and preprocess the CoNLL-2003 dataset, train the spaCy NER model, save the model, and evaluate its performance.
README.md: This file!
Running the Project
1. Training and Evaluating the NER Model
Simply run the training script:
Download the CoNLL-2003 dataset using the Hugging Face datasets library.
Convert the dataset into spaCy’s required format.
Initialize a blank English spaCy model, add the NER pipeline, and train it for 30 epochs (feel free to adjust the epoch count).
Save the trained model to disk (by default in /content/ner_model).
Load the model to make sample predictions and evaluate performance using Precision, Recall, and F1-Score metrics.

![image](https://github.com/user-attachments/assets/e384e402-1b90-4e5e-8794-e8a8da446d43)


This project showcases how to train a Named Entity Recognition (NER) model using spaCy and the CoNLL-2003 dataset. We’ll explore the steps for data preprocessing, model training, evaluation, and how to optionally serve your model via an API using FastAPI and ngrok.

1. Data Preprocessing & Feature Engineering
Dataset Loading

We use the Hugging Face Datasets library to load the conll2003 dataset. This provides train, validation, and test splits.
Token + Tag Extraction

Each sample in the dataset has tokens (words) and corresponding ner_tags.
The numeric ner_tags are mapped back to their string labels (e.g., B-PER, I-LOC, etc.).
spaCy Format Conversion

We convert each sentence into spaCy’s training format:
python
Copy
(text, {"entities": [(start_char, end_char, label), ...]})
The function convert_to_spacy_format loops through tokens, calculates character offsets, and groups them into entity spans. This ensures that B- and I- tags for the same entity are merged into one continuous span.
Feature Engineering

In a basic NER pipeline, the main “features” revolve around token embeddings, word shape, prefixes/suffixes, etc. SpaCy handles these features internally once we provide the raw text and labeled spans.
No additional external features (like POS tags or lexicons) were used, but you can easily extend this approach if you want more advanced feature engineering.4

2. Model Selection & Optimization Approach
Blank spaCy Model

We initialize a blank English model (nlp = spacy.blank("en")) and add the ner pipeline to it.
This means we’re starting from scratch—no pre-trained weights—so the model relies solely on the CoNLL-2003 training data.
Label Setup

We extract all entity labels from the dataset (excluding the “O” tag) and add them to the ner component via ner.add_label(label).
Training Loop

We create Example objects from our (text, entities) data.
We disable other spaCy components (if any) to focus only on training NER.
For ~30–35 epochs, we shuffle the training data, batch it (size=8), and call nlp.update(...) to optimize the model weights.
This iterative approach helps the model converge to a lower loss over time.
Hyperparameters

Epochs: 30 (can tweak if underfitting/overfitting).
Batch Size: 8 (small enough to avoid memory issues; can experiment).
Optimizer: spaCy’s default Adam-like optimizer.
Further optimization might involve:
Using a pre-trained model (e.g., en_core_web_sm) instead of a blank one.
Adjusting the learning rate or dropout.

3. Deployment Strategy
Local Deployment

Once the model is trained, it’s saved to disk (e.g., nlp.to_disk("/content/ner_model")).
You can load it anywhere using spacy.load("/content/ner_model") and run inference on new text.
FastAPI for Serving

Create a FastAPI app that loads your trained spaCy model.
Define an endpoint (e.g., GET /predict?text=...) to process incoming text.
Run the app locally with uvicorn main:app --host 0.0.0.0 --port 8000.
Ngrok for Public URL

Use pyngrok to expose your local server to the internet:
python
Copy
from pyngrok import ngrok
public_url = ngrok.connect(8000)
print("Public URL:", public_url)
This generates a temporary URL you can share with anyone for testing your NER endpoint in real time.
4. API Usage Guide
Assuming you’re running FastAPI on port 8000, and ngrok has given you a public URL like https://xyz.ngrok.io, you can test your NER endpoint with:

bash
Copy
curl -X GET "https://xyz.ngrok.io/predict?text=John%20lives%20in%20New%20York" \
     -H "Accept: application/json"
You’ll get a JSON response containing the recognized entities, for example:

json
Copy
{
  "entities": [
    ["John", "PER"],
    ["New York", "LOC"]
  ]
}

