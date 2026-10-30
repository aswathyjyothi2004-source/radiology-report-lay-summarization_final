# System Architecture

## 1. Overview

The project implements a text-to-text radiology report lay summarization system. Written radiology reports are processed by language models to generate patient-friendly summaries.

## 2. Dataset Input

The BioLaySumm dataset provides:

- Radiology reports
- Corresponding lay summaries

The radiology report is used as the model input, while the lay summary serves as the reference output during training and evaluation.

## 3. Model Comparison

The initial model comparison considered:

- Llama 3.2 1B
- Qwen 2.5 1.5B
- Phi-1

## 4. Baseline and Fine-Tuning Experiments

Llama 3.2 1B was used for the baseline experiment and QLoRA fine-tuning.

Fine-tuning experiments were performed using 1K, 2K, 5K and 10K training samples.

## 5. Retrieval-Augmented Generation

The RAG component uses a ChromaDB terminology knowledge base.

Relevant terminology information is retrieved and supplied to the generation process to support the production of lay summaries.

## 6. Explanation-Aware Knowledge Selection

The knowledge selection component identifies relevant terminology explanations for a given radiology report.

The selected explanations are incorporated into the generation process using the fine-tuned Llama 3.2 1B model.

## 7. Summary Generation

The language model generates a simplified summary from the radiology report and the relevant contextual knowledge.

## 8. Evaluation

Generated summaries are evaluated using automatic text-generation metrics, semantic similarity, readability measures and NLI-based automated hallucination estimation.

Error analysis is also used to examine generation issues.
