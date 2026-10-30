# Radiology Report Lay Summarization Using Parameter-Efficient Fine-Tuning and Explanation-Aware Knowledge Selection

Research code and documentation for **text-to-text** generation of patient-friendly lay summaries from written radiology reports. Medical images are not model inputs.

> **Artifact status:** This repository is a reproducibility-oriented project scaffold assembled from the manuscript and project details available at repository-preparation time. The original executable experiment notebooks, prediction files, and raw metric exports were not present in the accessible files. Templates are explicitly labelled; no missing execution history or results have been fabricated.

## Abstract

Radiology reports communicate findings using specialized terminology, compressed phrasing, and expressions of uncertainty that can be difficult for patients to interpret. This project studies patient-friendly summarization of written radiology reports using compact language models, parameter-efficient fine-tuning, retrieval-augmented generation (RAG), and explanation-aware terminology selection. The reported experimental programme compares Llama 3.2 1B, Qwen 2.5 1.5B, and Phi-1; fine-tunes Llama 3.2 1B Instruct with QLoRA using 1K, 2K, 5K, and 10K training examples; and investigates ChromaDB-based terminology retrieval and an explanation-aware selector (E4). Evaluation is intended to cover lexical overlap, semantic similarity, readability, and automated NLI-based unsupported-content estimation. These automatic measures are not clinical validation. No radiologist-based clinical evaluation is claimed.

## Research problem

The input is a written radiology report and the output is a patient-friendly lay summary. A useful summary should make terminology more accessible while preserving the report's findings, anatomy, laterality, negation, uncertainty, severity, and comparisons. A fluent but inaccurate summary can mislead; retrieval context can also be irrelevant or inappropriate. The project therefore compares adaptation and knowledge-selection strategies rather than assuming that more context always helps.

## Objectives

1. Compare compact language models for radiology-report lay summarization.
2. Establish a Llama baseline.
3. Study QLoRA fine-tuning at 1K, 2K, 5K, and 10K training-example scales.
4. Evaluate terminology-based RAG with a ChromaDB collection.
5. Investigate E4, an explanation-aware knowledge selector.
6. Evaluate generated summaries and inspect qualitative errors.

## Dataset

- Dataset identifier: [`BioLaySumm/BioLaySumm2025-LaymanRRG-opensource-track`](https://huggingface.co/datasets/BioLaySumm/BioLaySumm2025-LaymanRRG-opensource-track)
- Task: text-to-text radiology-report lay summarization.
- Source field documented in the manuscript: `radiology_report`.
- Reference field documented in the manuscript: `layman_report`.
- Dataset access, licensing, split definitions, and permitted redistribution must be checked against the dataset card before use.
- **Do not commit dataset files, patient-identifiable information, or restricted data.** The repository loads the dataset through Hugging Face Datasets; it does not bundle it.

## Methodology

### Model comparison and baseline

The documented initial comparison includes:
- `meta-llama/Llama-3.2-1B-Instruct`
- Qwen 2.5 1.5B
- Phi-1

The original comparison notebook was not available during repository preparation. `notebooks/01_model_comparison.ipynb` is therefore a planning/reproduction template, not an executed experiment.

### QLoRA fine-tuning

The primary generator is `meta-llama/Llama-3.2-1B-Instruct`. Historical settings recorded in the project brief/manuscript include 4-bit NF4 quantization, double quantization, FP16 compute, LoRA on `q_proj` and `v_proj`, rank 8, alpha 16, dropout 0.05, 3 epochs, learning rate `2e-4`, per-device batch size 1, gradient accumulation 4, maximum sequence length 384, seed 42, and maximum generation length 128 with 4 beams.

These are **historical documented settings, not a verified configuration for every run**. Confirm them against each original notebook/config before reproducing or claiming exact parity. See `configs/qlora_config.yaml`.

### Retrieval-augmented generation

The recorded ChromaDB collection is `biolaysumm_rrg_terms`, with embeddings from `sentence-transformers/all-MiniLM-L6-v2`. The manuscript describes sentence-level retrieval: exact term matching with exclusions, up to four exact matches per sentence, and semantic fallback at threshold 0.75 with up to two entries when no usable exact match exists. The retrieved definitions are auxiliary context; the radiology report remains the source of patient-specific findings.

The collection's provenance/version is not documented in the available record. The stored material must be described as terminology definitions, **not expert-verified or clinically reviewed lay explanations**.

### E4: explanation-aware knowledge selection

E4 is a learned selector configuration, not a new generator model. The available manuscript describes:
- Candidate report–concept–definition pairs constructed from the BioLaySumm training partition and ChromaDB metadata.
- 358,009 candidate pairs in the recorded run.
- Similarity-derived weak labels using `all-MiniLM-L6-v2` and a cosine threshold of 0.40.
- A DistilBERT two-class classifier (`distilbert-base-uncased`).
- Candidate-row 80/20 stratified validation, not report-disjoint validation.
- Up to three selected definitions per report in the described post-selection step.

The weak labels are proxy labels, not expert correctness judgments. A 300-example manual-review worksheet is described, but the number of completed judgments and inter-annotator agreement are not established. The candidate-row split can place candidates from the same report in both partitions, so report-level generalization is not established.

## System architecture

```text
Written radiology report
          |
          v
Input validation / sentence segmentation
          |
          +-------------------------------+
          |                               |
          v                               v
Fine-tuned Llama generator          Terminology knowledge base
          |                         (ChromaDB; MiniLM embeddings)
          |                               |
          |                         Retrieval / E4 selection
          |                               |
          +---------------+---------------+
                          |
                          v
             Context-conditioned generation
                          |
                          v
              Patient-friendly lay summary
                          |
                          v
        Automatic evaluation + error inspection
```

The diagram is conceptual. It does not imply that all experiment variants use the same retrieval or generation path.

## Experimental setup

The recorded experiment families are:
- Initial compact-model comparison: Llama 3.2 1B, Qwen 2.5 1.5B, Phi-1.
- Llama baseline evaluation.
- QLoRA fine-tuning: 1K, 2K, 5K, 10K training examples.
- RAG variants using ChromaDB terminology definitions.
- E4 explanation-aware knowledge selection.
- Final evaluation and qualitative error analysis.

The available manuscript reports a 1K setup with 1,000 training, 100 validation, and 100 test examples. The corresponding counts and report-level overlap checks for all other configurations were not retained in the available manuscript. Do not assume the same held-out reports were used across all configurations without verifying the source artifacts.

## Evaluation metrics

| Category | Metrics |
|---|---|
| Lexical overlap | BLEU, ROUGE-1, ROUGE-2, ROUGE-L, METEOR |
| Contextual similarity | BERTScore precision, recall, F1 |
| Embedding similarity | Semantic similarity (implementation must be documented) |
| Readability | Flesch-Kincaid Grade Level, Dale-Chall, Coleman-Liau |
| Unsupported-content estimate | NLI-based automated estimate |

An NLI model's non-entailment prediction is an **automated unsupported-content signal**, not proof of hallucination and not clinical validation. The metric's premise/hypothesis construction, label mapping, thresholds, and aggregation must be specified before reporting it.

## Results

No raw prediction files or metric exports were found among the accessible project files when this repository was assembled. Consequently, this repository does **not** publish a verified result table or claim that any historical metric was independently reproduced.

The manuscript file preserved under `paper/manuscript.tex` contains manuscript-reported aggregate values and explicitly discusses inconsistencies and missing provenance. Treat those values as **unverified manuscript content**, not as validated benchmark results, until they are reconciled against the original prediction and evaluation files. In particular, the manuscript flags an internal BERTScore precision/recall/F1 inconsistency and an unusually high METEOR value for the 10K fine-tuned configuration.

Add verified outputs only after checking:
1. Exact evaluation examples/report identifiers.
2. Reference and generated text alignment.
3. Metric library and version.
4. Tokenization/normalization settings.
5. Generation parameters.
6. Per-example predictions and aggregate recomputation.

## Installation

Python 3.10–3.12 is recommended. GPU and package compatibility depend on the selected PyTorch/CUDA environment.

```bash
git clone <YOUR-REPOSITORY-URL>
cd radiology-report-lay-summarization

python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
pip install -e ".[dev]"
```

For a GPU-enabled QLoRA environment, install a PyTorch build compatible with the host CUDA runtime before installing the optional training dependencies. BitsAndBytes support is platform-dependent and is not guaranteed on macOS/Apple Silicon.

Copy `.env.example` to `.env` only if needed. Never commit `.env` or access tokens.

## Usage

### Load the dataset

```python
from radiology_summarization.data_loader import load_biolaysumm

dataset = load_biolaysumm()
print(dataset)
```

### Validate the environment/config

```bash
python -m radiology_summarization.cli check-config configs/qlora_config.yaml
```

### Run notebooks

Open the notebook templates in Jupyter or Google Colab. They contain explicit TODO cells and provenance checks. They do not contain original execution history. Replace each template section with the corresponding verified code from the original experiment before treating it as a reproduction.

### Generate and evaluate

The modular source contains reusable components and explicit extension points. Some model-specific steps remain intentionally incomplete because the original prompts, preprocessing, trainer/collator, checkpoint layout, and evaluation scripts were not available. See module docstrings and `docs/experimental_setup.md`.

## Repository structure

```text
radiology-report-lay-summarization/
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── pyproject.toml
├── .gitignore
├── .env.example
├── notebooks/                 # Seven clearly labelled templates
├── src/radiology_summarization/
├── configs/                   # Historical settings vs illustrative defaults labelled
├── results/                   # Empty experiment folders with README instructions
├── docs/
└── paper/                      # Preserved manuscript source and figure placeholders
```

## Limitations and responsible use

- No clinical evaluation or radiologist review was performed.
- NLI-based estimates are automated proxies and must not be described as clinical validation.
- The ChromaDB knowledge base is not established as expert-verified.
- Dataset/report provenance beyond the named public dataset is not established in the available record.
- The selector's candidate-row validation split is not report-disjoint.
- Some experiment configurations, data splits, generation parameters, metric implementations, and aggregate values require verification against original artifacts.
- Do not use generated summaries for diagnosis, treatment, triage, or patient-care decisions.
- Protect patient privacy and comply with dataset/model licenses and institutional requirements.

## Citation

See `CITATION.cff`. The paper's existing bibliography is preserved inside `paper/manuscript.tex`; `paper/references.bib` is a placeholder because no verified BibTeX source file was found. Do not cite this repository as containing fully reproducible experiments until the missing original code and outputs are added.
