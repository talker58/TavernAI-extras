# TavernAI - Extras
## What is this
A set of unofficial APIs for various [TavernAI](https://github.com/TavernAI/TavernAI) extensions

## How to run
* Install Python 3.10
* Run `pip install -r requirements.txt`
* Run `python server.py`

## Included functionality
### BLIP model for image captioning
`POST /api/caption`
#### **Input**
```
{ "image": "base64 encoded image" }
```
#### **Output**
```
{ "caption": "caption of the posted image" }
```

### BART model for text summarization
`POST /api/summarize`
#### **Input**
```
{ "text": "text to be summarize", "params": {} }
```
#### **Output**
```
{ "summary": "summarized text" }
```
#### Optional: `params` object for control over summarization:
| Name                  | Default value                                                 |
| --------------------- | ------------------------------------------------------------- |
| `temperature`         | 1.0                                                           |
| `repetition_penalty`  | 1.0                                                           |
| `max_length`          | 500                                                           |
| `min_length`          | 200                                                           |
| `length_penalty`      | 1.5                                                           |
| `bad_words`           | ["\n", '"', "*", "[", "]", "{", "}", ":", "(", ")", "<", ">"] |

### BERT model for text classification
`POST /api/classify`
#### **Input**
```
{ "text": "text to classify sentiment of" }
```
#### **Output**
```
{
    "classification": [
        {
            "label": "joy",
            "score": 1.0
        },
        {
            "label": "anger",
            "score": 0.7
        },
        {
            "label": "love",
            "score": 0.6
        },
        {
            "label": "sadness",
            "score": 0.5
        },
        {
            "label": "fear",
            "score": 0.4
        },
        {
            "label": "surprise",
            "score": 0.3
        }
    ]
}
```
> **NOTES**
> 1. Sorted by descending score order
> 2. Six fixed categories
> 3. Value range from 0.0 to 1.0

### Key phrase extraction
`POST /api/keywords`
#### **Input**
```
{ "text": "text to be scanned for key phrases" }
```
#### **Output**
```
{
    "keywords": [
        "array of",
        "extracted",
        "keywords",
    ]
}
```

### GPT-2 for Stable Diffusion prompt generation
`POST /api/prompt`
#### **Input**
```
{ "name": "character name (optional)", "text": "textual summary of a character" }
```
#### **Output**
```
{ "prompts": [ "array of generated prompts" ] }
```

### Stable Diffusion for image generation
`POST /api/image`
#### **Input**
```
{ "prompt": "prompt to be generated" }
```
#### **Output**
```
{ "image": "base64 encoded image" }
```

## Additional options
| Flag                     | Description                                                            |
| ------------------------ | ---------------------------------------------------------------------- |
| `--port`                 | Specify the port on which the application is hosted. Default: **5100** |
| `--listen`               | Hosts the app on the local network                                     |
| `--share`                | Shares the app on CloudFlare tunnel                                    |
| `--cpu`                  | Run the models on the CPU instead of CUDA                              |
| `--summarization-model`  | Load a custom BART summarization model.<br>Expects a HuggingFace model ID.<br>Default: [Qiliang/bart-large-cnn-samsum-ChatGPT_v3](https://huggingface.co/Qiliang/bart-large-cnn-samsum-ChatGPT_v3) |
| `--classification-model` | Load a custom BERT sentiment classification model.<br>Expects a HuggingFace model ID.<br>Default: [bhadresh-savani/distilbert-base-uncased-emotion](https://huggingface.co/bhadresh-savani/distilbert-base-uncased-emotion) |
| `--captioning-model`     | Load a custom BLIP captioning model.<br>Expects a HuggingFace model ID.<br>Default: [Salesforce/blip-image-captioning-base](https://huggingface.co/Salesforce/blip-image-captioning-base) |
| `--keyphrase-model`      | Load a custom key phrase extraction model.<br>Expects a HuggingFace model ID.<br>Default: [ml6team/keyphrase-extraction-distilbert-inspec](https://huggingface.co/ml6team/keyphrase-extraction-distilbert-inspec) |
| `--prompt-model`         | Load a custom GPT-2 prompt generation model.<br>Expects a HuggingFace model ID.<br>Default: [FredZhang7/anime-anything-promptgen-v2](https://huggingface.co/FredZhang7/anime-anything-promptgen-v2) |
| `--sd-model`             | Load a custom Stable Diffusion image generation model.<br>Expects a HuggingFace model ID.<br>Default: [ckpt/anything-v4.5-vae-swapped](https://huggingface.co/ckpt/anything-v4.5-vae-swapped)<br>*Must have VAE pre-baked in PyTorch format or the output will look drab!* |
| `--sd-cpu`               | Forces the Stable Diffusion generation pipeline to run on the CPU.<br>**SLOW!** |