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

