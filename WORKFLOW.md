# LoRA JSON Project Workflow

## Branch Structure

### 1. `dataset-generation` ✅ (COMPLETED)
**Goal**: Generate synthetic training/eval datasets
**Where**: Run locally (no GPU needed)
**Output**: `data/train.jsonl`, `data/eval.jsonl`

**Steps**:
- Run `python generate_dataset.py`
- Commit dataset to git
- Push branch

---

### 2. `baseline-eval` (CURRENT)
**Goal**: Benchmark Mistral 7B baseline (no LoRA)
**Where**: Google Colab (needs GPU - T4 or better)
**Output**: `baseline_results.json`

**Steps**:
1. Push this branch to GitHub
2. Open Colab: https://colab.research.google.com
3. Upload `notebooks/02_baseline_eval.ipynb`
4. Runtime → Change runtime type → GPU (T4)
5. Update repo URL in cell 2 (replace `YOUR_USERNAME`)
6. Run all cells
7. Download `baseline_results.json`
8. Commit locally and push

**Expected metrics** (baseline):
- Valid JSON: ~70-85%
- Schema compliance: ~60-75%
- Intent accuracy: ~85-90%

---

### 3. `lora-training` (NEXT)
**Goal**: Fine-tune Mistral 7B with QLoRA
**Where**: Google Colab (needs GPU - T4 minimum, A100 better)
**Output**: LoRA adapter weights

**Steps**:
1. Load `data/train.jsonl`
2. Use QLoRA (4-bit + LoRA adapters)
3. Train for 1-3 epochs (~30-60 min on T4)
4. Save adapter to HuggingFace Hub or commit to repo
5. Push branch

**Hyperparameters**:
- LoRA rank: 16
- LoRA alpha: 32
- Learning rate: 2e-4
- Batch size: 4 (with gradient accumulation)
- Max steps: ~1500

---

### 4. `lora-eval` (FINAL)
**Goal**: Benchmark Mistral 7B + LoRA adapter
**Where**: Google Colab (needs GPU)
**Output**: `lora_results.json`

**Steps**:
1. Load base model + LoRA adapter
2. Run same evaluation as baseline
3. Compare metrics

**Expected improvements**:
- Valid JSON: ~95-99% ✨
- Schema compliance: ~95-98% ✨
- Intent accuracy: ~92-95%

---

## Quick Commands

### Dataset Generation (local)
```bash
git checkout dataset-generation
python generate_dataset.py
git add data/ generate_dataset.py
git commit -m "Generate dataset"
git push
```

### Baseline Eval (Colab)
```bash
git checkout baseline-eval
git push origin baseline-eval
# Then run in Colab
# Download results
git add baseline_results.json
git commit -m "Baseline eval results"
git push
```

### LoRA Training (Colab)
```bash
git checkout lora-training
git push origin lora-training
# Run training in Colab
# Save adapter
git commit -m "LoRA adapter"
git push
```

### LoRA Eval (Colab)
```bash
git checkout lora-eval
git push origin lora-eval
# Run eval in Colab
git add lora_results.json
git commit -m "LoRA eval results"
git push
```

---

## Colab GPU Access

**Free Tier**: T4 GPU (15GB VRAM)
- Sufficient for 4-bit quantization
- ~30-60 min training time

**Colab Pro** ($10/month): A100 GPU (40GB VRAM)
- Faster training (~10-15 min)
- Can use higher batch sizes

---

## Metrics to Track

| Metric | Baseline | LoRA Target |
|--------|----------|-------------|
| JSON Validity | 70-85% | **95-99%** |
| Schema Compliance | 60-75% | **95-98%** |
| Intent Accuracy | 85-90% | **92-95%** |
| Entity Extraction | 75-85% | **88-93%** |

The key improvement is **schema compliance** - smaller models struggle with strict JSON formatting, LoRA fixes this.
