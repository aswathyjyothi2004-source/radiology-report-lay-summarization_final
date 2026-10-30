# Methodology

## 1. Task Definition

This project focuses on generating patient-friendly lay summaries from written radiology reports. It follows a text-to-text summarization approach. The radiology report is provided as input, and the model generates a simplified summary as output. Medical images are not used as model inputs.

## 2. Dataset

The experiments use the BioLaySumm 2025 Layman Radiology Report Generation dataset:

`BioLaySumm/BioLaySumm2025-LaymanRRG-opensource-track`

The dataset provides radiology reports and their corresponding lay summaries.

## 3. Model Comparison

Compact language models were examined to identify a suitable model for the summarization task. The comparison included Llama 3.2 1B, Qwen 2.5 1.5B, and Phi-1.

## 4. Baseline and Fine-Tuning

A baseline experiment was conducted using Llama 3.2 1B. Parameter-efficient fine-tuning was then performed using QLoRA with different training dataset sizes: 1K, 2K, 5K, and 10K samples.

## 5. Retrieval-Augmented Generation

A retrieval-augmented generation approach was implemented using a ChromaDB-based terminology knowledge base. Relevant terminology information was retrieved and incorporated into the summarization process.

## 6. Explanation-Aware Knowledge Selection

An explanation-aware knowledge selection approach was developed to select relevant terminology explanations for radiology reports. The selected knowledge was incorporated into the generation process using the fine-tuned language model.

## 7. Evaluation

The generated summaries were evaluated using automatic text-generation, semantic similarity, readability, and NLI-based measures. Error analysis was also conducted to examine issues in generated summaries.
