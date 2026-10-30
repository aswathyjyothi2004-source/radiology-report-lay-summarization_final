# Experimental Setup

## 1. Dataset

**Dataset:** BioLaySumm 2025 Layman Radiology Report Generation

**Hugging Face identifier:**
`BioLaySumm/BioLaySumm2025-LaymanRRG-opensource-track`

The task uses written radiology reports as input and corresponding lay summaries as reference outputs.

## 2. Models

The model comparison included:

- Llama 3.2 1B
- Qwen 2.5 1.5B
- Phi-1

Llama 3.2 1B was used for the baseline and subsequent fine-tuning experiments.

## 3. Fine-Tuning Configuration

The fine-tuning experiments used:

- Model: `meta-llama/Llama-3.2-1B-Instruct`
- Fine-tuning method: QLoRA
- Quantization: 4-bit NF4
- Double quantization: Enabled
- Computation data type: FP16
- LoRA target modules: `q_proj`, `v_proj`
- LoRA rank: 8
- LoRA alpha: 16
- LoRA dropout: 0.05
- Training epochs: 3
- Learning rate: 2e-4
- Batch size: 1
- Gradient accumulation steps: 4
- Maximum sequence length: 384
- Random seed: 42

## 4. Training Dataset Sizes

Fine-tuning experiments were conducted using four training dataset sizes:

| Experiment | Training samples |
|---|---:|
| 1K | 1,000 |
| 2K | 2,000 |
| 5K | 5,000 |
| 10K | 10,000 |

## 5. Retrieval Configuration

The retrieval component used:

- Vector database: ChromaDB
- Embedding model: `sentence-transformers/all-MiniLM-L6-v2`
- Collection: `biolaysumm_rrg_terms`

The knowledge base contains radiology terminology and lay explanations.

## 6. Evaluation Measures

The evaluation included:

- BLEU
- ROUGE-1, ROUGE-2 and ROUGE-L
- METEOR
- BERTScore
- Semantic similarity
- Flesch-Kincaid Grade Level
- Dale-Chall readability
- Coleman-Liau Index
- NLI-based automated hallucination estimation

The exact evaluation settings and dataset subsets should be taken from the corresponding experiment notebooks.
