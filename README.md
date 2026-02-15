# LoRA for Structured JSON Output

Testing whether LoRA fine-tuning can make smaller open-source models reliably output structured JSON. Spoiler: it works pretty well.

## The Problem

Getting smaller LLMs (like Mistral 7B) to consistently follow strict JSON schemas is hard. Even with detailed prompts, they'll sometimes break the format, especially on messy or adversarial inputs.

## The Solution

Fine-tune with QLoRA on a synthetic dataset of customer support messages. The model learns to always output valid, schema-compliant JSON regardless of input quality.

## Dataset

- **6000 training examples**: Customer support messages (refunds, cancellations, billing issues, etc.)
- **800 eval examples**: Intentionally harder - multi-intent messages, prompt injections, missing info, typos

All synthetic, generated with Python (no LLM calls needed).

## Results

Evaluated on 100 challenging examples:

| Metric | Baseline | + LoRA | Improvement |
|--------|----------|--------|-------------|
| Valid JSON | 100% | 100% | - |
| Schema compliance | 87% | **100%** | +13% |
| Intent accuracy | 84% | **98%** | +14% |

The baseline model already produced valid JSON, but often got the schema wrong (missing keys, wrong types, extra fields). LoRA fixed this completely.

## Try It Yourself

The LoRA adapter is available in two places:
- **HuggingFace Hub**: [aashnakunk/mistral-7b-json-support](https://huggingface.co/aashnakunk/mistral-7b-json-support)
- **This repo**: `mistral_lora_adapter.zip`

Check the notebooks in `notebooks/` to see the full pipeline:
1. `01_generate_dataset.ipynb` - Dataset generation
2. `02_baseline_eval.ipynb` - Baseline benchmark
3. `03_lora_training.ipynb` - QLoRA training
4. `04_lora_eval.ipynb` - LoRA evaluation

## Model Details

- Base: Mistral 7B Instruct v0.3
- Quantization: 4-bit (QLoRA)
- LoRA rank: 16
- Training time: ~2 hrs on T4 GPU

## Why This Matters

Structured output is critical for LLM applications (tool calling, database updates, API integrations). This shows you can make 7B models nearly 100% reliable with just a few thousand examples and minimal compute.
