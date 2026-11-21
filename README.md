# LoRA Fine-Tuning for Intent Detection and Slot Filling

## Project Summary

This project uses **LoRA (Low-Rank Adaptation)** to fine-tune a small language model, **Qwen2.5-0.5B-Instruct**, for natural language understanding in a voice assistant. Given a user command, the model generates a JSON object containing the **intent** of the command and the **slots** (key pieces of information) it mentions:

```
Input : What will the weather be this year in Horseshoe Lake State Fish and Wildlife Area?
Output: {"intent": "GetWeather", "slots": {"timeRange": "this year", "geographic_poi": "Horseshoe Lake State Fish and Wildlife Area"}}
```

LoRA freezes the original model weights and trains only small low-rank matrices added to the attention layers, so fine-tuning fits on a single free GPU. The experiments will compare:

- the **base model** (zero-shot, no fine-tuning) against the **LoRA fine-tuned** model
- different **LoRA ranks** (r = 8, 16, 32): accuracy, number of trainable parameters, and training time

**Evaluation metrics:** JSON validity rate, intent accuracy, and slot precision / recall / F1 (a slot counts as correct only if both its name and value match).

## Team

| Name | GitHub username |
|---|---|
| Zhiyu Hao | [@zyuhao23](https://github.com/zyuhao23) |

## Dataset

**SNIPS NLU Benchmark** (2017-06 custom intent engines)

- Original source: https://github.com/sonos/nlu-benchmark/tree/master/2017-06-custom-intent-engines
- License: CC0-1.0
- 14,484 crowdsourced English voice-assistant commands across 7 intents: `AddToPlaylist`, `BookRestaurant`, `GetWeather`, `PlayMusic`, `RateBook`, `SearchCreativeWork`, `SearchScreeningEvent`
- Every sentence is human-annotated with its intent and its slots (39 slot types, e.g. `city`, `timeRange`, `artist`, `restaurant_type`)
- Split used: the official training files (about 1,950 sentences per intent) for training, and the official validation files (100 sentences per intent, 700 total) as the held-out test set

Citation, as requested by the dataset authors:

> Coucke A. et al., "Snips Voice Platform: an embedded Spoken Language Understanding system for private-by-design voice interfaces." 2018. https://arxiv.org/abs/1805.10190
