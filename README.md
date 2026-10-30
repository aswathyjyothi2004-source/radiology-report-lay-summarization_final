# Radiology Report Lay Summarization Using Parameter-Efficient Fine-Tuning and Explanation-Aware Knowledge Selection

## Abstract

This project investigates the generation of patient-friendly summaries from written radiology reports using compact language models and parameter-efficient fine-tuning techniques. It explores baseline language model performance, QLoRA-based fine-tuning, retrieval-augmented generation (RAG), and explanation-aware knowledge selection. The generated summaries are evaluated using text-generation, semantic similarity, readability, and automated natural language inference metrics.

## 1. Introduction

Radiology reports contain medical terminology and technical descriptions that may be difficult for non-specialist readers to understand. This project focuses on transforming written radiology reports into simpler, patient-friendly summaries while preserving the information expressed in the original reports.

The task is formulated as text-to-text summarization. The written radiology report is provided as input, and a lay summary is generated as output. Medical images are not used as model inputs.

## 2. Objectives

- Generate understandable lay summaries from radiology reports.
- Compare compact language models for the summarization task.
- Investigate parameter-efficient fine-tuning using QLoRA.
- Explore retrieval-augmented generation using a terminology knowledge base.
- Investigate explanation-aware knowledge selection.
- Evaluate generated summaries using automatic evaluation metrics.

## 3. Dataset

The project uses the BioLaySumm dataset:

`BioLaySumm/BioLaySumm2025-LaymanRRG-opensource-track`

The dataset contains radiology reports and corresponding lay summaries. The original dataset and its associated files are not included in this repository. Users must obtain access according to the dataset's terms.

## 4. Models

The compact language models investigated include:

- Llama 3.2 1B
- Qwen 2.5 1.5B
- Phi-1

Llama 3.2 1B is used for the main fine-tuning and knowledge-enhanced experiments.

## 5. Methodology

### 5.1 Baseline Model Comparison

Compact language models are investigated to understand their performance on radiology report lay summarization.

### 5.2 QLoRA Fine-Tuning

Parameter-efficient fine-tuning is performed using QLoRA with the Llama 3.2 1B model. Experiments investigate different training-data sizes: 1K, 2K, 5K, and 10K.

### 5.3 Retrieval-Augmented Generation

A ChromaDB-based terminology knowledge base is used to retrieve relevant explanations that can support lay-summary generation.

### 5.4 Explanation-Aware Knowledge Selection (E4)

An explanation-aware knowledge-selection approach is investigated to identify useful terminology explanations before generating the final lay summary.

### 5.5 Evaluation

Generated summaries are evaluated using automatic text-generation, semantic similarity, readability, and NLI-based measures.

## 6. Evaluation Metrics

The evaluation includes:

- BLEU
- ROUGE-1
- ROUGE-2
- ROUGE-L
- METEOR
- BERTScore Precision, Recall, and F1
- Semantic Similarity
- Flesch-Kincaid Grade Level
- Dale-Chall Readability
- Coleman-Liau Index
- NLI-based automated hallucination estimation

Automated NLI estimates are used as an evaluation proxy and do not constitute clinical validation.

## 7. Repository Structure

```text
radiology-report-lay-summarization/
├── notebooks/
├── src/
├── configs/
├── results/
├── docs/
├── paper/
├── README.md
├── requirements.txt
├── LICENSE
└── CITATION.cff
```

## 8. Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/radiology-report-lay-summarization.git
cd radiology-report-lay-summarization
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Additional access to the dataset and gated model repositories may be required.

## 9. Reproducibility

The notebooks document the experimental workflow. Reproduction requires the appropriate dataset access, model access, dependencies, configuration settings, and experiment outputs.

The exact configuration and evaluation settings should be consulted in the corresponding experiment files.

## 10. Limitations

- The system performs text-to-text summarization and does not interpret medical images.
- Automatic evaluation metrics do not establish clinical correctness.
- NLI-based hallucination estimation is an automated proxy.
- Clinical evaluation was not performed.
- The terminology knowledge base should not be considered expert-verified unless independently validated.

## 11. Citation

Citation information will be updated when the associated research paper is finalized and publicly available.

## 12. Acknowledgements

This project uses the BioLaySumm dataset and open-source language-model and machine-learning libraries. Please refer to their original documentation and licensing terms.
