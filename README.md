# LoRA Structured JSON Reliability Project

This project demonstrates how LoRA fine-tuning improves structured JSON output reliability in smaller open-source LLMs (Mistral 7B).

## Goal
Convert messy customer support messages into strict schema-compliant JSON.

## Why LoRA?
Prompt-only schema enforcement works inconsistently in smaller models. LoRA improves schema reliability by adapting internal generation patterns.

## Steps
1. Generate synthetic dataset
2. Benchmark baseline model
3. Train LoRA adapter
4. Benchmark LoRA model
5. Compare schema validity and accuracy

## Model
Mistral 7B Instruct + QLoRA

## Metrics
- JSON validity rate
- Schema compliance rate
- Intent classification accuracy
- Entity extraction accuracy
