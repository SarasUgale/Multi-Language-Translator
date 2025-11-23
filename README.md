# Multi-Language-Translator
🌐 Multi-Language Translator (English → Multiple Languages)

A simple, fast, and accurate English-to-Multilingual Translator built using:

HuggingFace Transformers

MarianMT translation models

Gradio Web UI

This app can translate English text into French, German, Hindi, Spanish, and Japanese with a beautifully interactive UI.

🚀 Features

✔️ 1. Supports 5 Major Languages

Translate English sentences into:

🇫🇷 French

🇩🇪 German

🇮🇳 Hindi

🇪🇸 Spanish

🇯🇵 Japanese

✔️ 2. Pretrained Transformer Models

Uses high-quality Helsinki-NLP MarianMT models for accurate translations.

✔️ 3. Fast Performance with Model Caching

Models load only once → Faster repeated translations.

✔️ 4. Beautiful & User-Friendly Interface

Powered by Gradio, making it easy to translate directly in the browser.

✔️ 5. One-Click Demo Sharing

Supports share=True to create a public URL instantly.


🧠 How It Works

User enters English text.

Selects a target language from the dropdown.

App loads the corresponding MarianMT model (cached after first load).

Text is tokenized → translated → decoded.

The translated output is displayed instantly.

📦 Installation
1. Install Dependencies
```
pip install gradio transformers sentencepiece
```
▶️ Run the App

Copy the following script into a Python file or Google Colab cell and run:
```
!pip install -q gradio transformers sentencepiece

import gradio as gr
from transformers import MarianTokenizer, MarianMTModel

language_models = {
    "French": "Helsinki-NLP/opus-mt-en-fr",
    "German": "Helsinki-NLP/opus-mt-en-de",
    "Hindi": "Helsinki-NLP/opus-mt-en-hi",
    "Spanish": "Helsinki-NLP/opus-mt-en-es",
    "Japanese": "Helsinki-NLP/opus-mt-en-jap"
}

loaded_models = {}

def translate(text, target_language):
    if not text.strip():
        return "❗ Please enter some English text."

    model_name = language_models[target_language]

    if target_language not in loaded_models:
        tokenizer = MarianTokenizer.from_pretrained(model_name)
        model = MarianMTModel.from_pretrained(model_name)
        loaded_models[target_language] = (tokenizer, model)

    tokenizer, model = loaded_models[target_language]

    inputs = tokenizer(text, return_tensors="pt", padding=True, truncation=True)
    translated = model.generate(**inputs)
    return tokenizer.decode(translated[0], skip_special_tokens=True)

iface = gr.Interface(
    fn=translate,
    inputs=[
        gr.Textbox(label="Enter English Sentence"),
        gr.Dropdown(label="Translate To", choices=list(language_models.keys()), value="French")
    ],
    outputs=gr.Textbox(label="Translated Sentence"),
    title="🌐 Multi-Language Translator",
    description="Translate English sentences to French, German, Hindi, Spanish, or Japanese using a pre-trained Transformer model."
)

iface.launch(share=True)
```
📁 Project Structure
```
📦 Multi-Language-Translator
 ├── translator.py          # Main translation code
 ├── README.md              # Documentation
 └── requirements.txt       # Libraries used
```
📜 Requirements.txt
```
gradio
transformers
sentencepiece
torch
```
🌍 Demo 

🔗 Live Demo: https://f947357a6c9c8662b1.gradio.live/

🤝 Contributing

Pull requests are welcome!
Feel free to:

add more languages

improve UI

add speech-to-text support
