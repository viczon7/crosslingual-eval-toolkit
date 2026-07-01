# crosslingual-eval-toolkit

Toolkit for adapting and evaluating multiple-choice commonsense benchmarks across languages, with preprocessing, postprocessing, multilingual dataset variants, model evaluation, and comparative error analysis.

## Overview

This repository contains code and data for cross-lingual benchmark adaptation and evaluation. The current version focuses on a 500-item HellaSwag validation subset and compares model performance across English and Russian benchmark variants.

The project supports:
- sampling a benchmark subset,
- translating benchmark items into target languages,
- normalizing structural markers after translation,
- incorporating manually post-edited benchmark versions,
- evaluating a language model on each benchmark variant,
- and comparing results across languages.

## Research question

How does model performance change when a benchmark is adapted across languages?

This repository is designed as a reusable starting point for cross-lingual evaluation workflows.

## Repository structure

```text
crosslingual-eval-toolkit/
├── README.md
├── requirements.txt
├── .gitignore
├── src/
│   ├── sample_subset.py
│   ├── translate.py
│   ├── postprocess.py
│   └── evaluate.py
├── data/
│   ├── raw/
│   │   ├── hellaswag_val_500_en.jsonl
│   │   ├── hellaswag_val_500_ru_mt.jsonl
│   └── processed/
│       ├── hellaswag_val_500_ru_normalized.jsonl
│       ├── hellaswag_val_500_ru_postedited.jsonl
└── docs/
    ├── post_editing_ru.pdf
    └── figures/
        └── graph_russian_old_vs_new.jpg

```

## Included files

### Scripts
- `sample_subset.py` samples a benchmark subset from HellaSwag.
- `translate.py` translates the English benchmark subset into target languages while preserving JSONL structure.
- `postprocess.py` normalizes structural markers such as `[header]`, `[title]`, `[step]`, and `[substeps]`.
- `evaluate.py` evaluates a model on each benchmark file and reports accuracy.

### Data
- `hellaswag_val_500_en.jsonl` is the original English subset.
- `hellaswag_val_500_ru_mt.jsonl` is the machine-translated benchmark variant.
- `hellaswag_val_500_ru_normalized.jsonl` is the Russian version after marker normalization.
- `hellaswag_val_500_ru_postedited.jsonl` and the manually post-edited variant.

### Documentation
- `post_editing_ru.pdf` documents translation or post-editing issues in Russian.
- `docs/figures/graph_russian.jpg` shows an example comparison between two Russian benchmark variants.

## Installation

### Requirements
- Python 3.10+
- `transformers`
- `torch`
- `deepl`
- additional dependencies listed in `requirements.txt`

### Setup

```bash
git clone https://github.com/viczon7/crosslingual-eval-toolkit.git
cd crosslingual-eval-toolkit
pip install -r requirements.txt
```

## Usage

### 1. Sample the English subset

```bash
python src/sample_subset.py
```

### 2. Translate the benchmark

Set your DeepL API key:

```bash
export DEEPL_API_KEY=your_key_here
```

Then run:

```bash
python src/translate.py
```

### 3. Normalize structural markers

```bash
python src/postprocess.py
```

### 4. Evaluate the benchmark variants

```bash
python src/evaluate.py
```

The evaluation script reports accuracy for each JSONL benchmark file.

## Example workflow

A typical workflow is:

1. Sample an English benchmark subset.
2. Translate it into one or more target languages.
3. Normalize or repair structural markers introduced during translation.
4. Optionally create manually post-edited benchmark variants.
5. Run evaluation on the English, machine-translated, normalized, and post-edited files.
6. Compare performance across benchmark variants.

## Results

The figure below shows an example comparison between two Russian benchmark variants.

![Russian benchmark variant comparison](docs/figures/graph_russian.jpg)

This kind of comparison helps show how benchmark adaptation decisions, including cleanup and revision of translated items, can affect downstream model performance.

The current experiments compare English with two Russian benchmark variants: an earlier Russian version and a revised Russian version.

| Benchmark variant | Accuracy |
|---|---:|
| English | `0.674.xxx` |
| Russian (Old) | `0.476` |
| Russian (New) | `0.516` |

The English benchmark remains the strongest reference, while the manually post-edited Russian version improves over the raw machine-translated Russian variant and reduces the performance gap.

## Current benchmark setting

The current project setup evaluates a 500-item HellaSwag validation subset across:
- English,
- machine-translated Russian,
- normalized Russian,
- and post-edited Russian.

## Motivation

Cross-lingual benchmark adaptation is not neutral: translation quality, formatting artifacts, and post-editing choices can change the difficulty and behavior of a benchmark. This repository is intended to help study those effects in a structured and reproducible way.

## Future improvements

Possible extensions include:
- support for more benchmarks,
- support for more languages,
- config-based experiment runs,
- richer evaluation metrics beyond accuracy,
- automatic error analysis reports,
- and visualization of cross-lingual performance differences.

## Citation

If you use this repository, please cite the benchmark and model sources used in the project.

- Zellers et al. (2019), *HellaSwag: Can a Machine Really Finish Your Sentence?*
- Gemma Team (2024), *Gemma 2: Improving Open Language Models at a Practical Size.*
