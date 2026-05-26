# Facet Generation for Question Disambiguation

> Fine-tuning **Qwen 2.5 7B** with SFT + DPO to generate high-quality disambiguation facets, evaluated by Gemini 2.5 Flash — CS 546 @ UIUC.

[![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python)](https://python.org)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow?logo=huggingface)](https://huggingface.co)

## What This Does

When a question is ambiguous ("How do I make pasta?"), multiple valid interpretations exist. This project trains an LLM to surface *which* interpretation a user intends by generating **facets** — structured disambiguation options — classified into a three-category taxonomy.

## Ambiguity Taxonomy

| Category | Example |
|----------|---------|
| **Entity Reference** | "Apple" → tech company vs. fruit |
| **Underspecified Common Noun** | "teacher" → which subject, which grade level? |
| **Degree of an Action** | "help" → light guidance vs. complete solution? |

## File Guide

| File | What it does |
|------|--------------|
| `Untitled (1).ipynb` | **Data preparation** — loads dataset, applies backwards filtering (verifies generated facets can explain gold QA pairs), curates training set |
| `cs546_sft_dpo (1).ipynb` | **Main training notebook** — Stage 2 SFT on curated facet data; Stage 3 iterative DPO with multi-criteria Gemini scoring |
| `CS546_Final_Report-Bhoomika-2 (1).pdf` | Full research report: methodology, ablations, and results |

## Three-Stage Training Pipeline

```
Stage 1: Data Preparation        Stage 2: SFT                  Stage 3: DPO
─────────────────────────────    ──────────────────────────    ─────────────────────────────
Generate facet candidates         Fine-tune Qwen 2.5 7B on      Generate N hypotheses per
→ Backwards filter validation     taxonomy-aligned facet data   question → Gemini scores
  (can facets explain gold QA?)   → Improve ambiguity detection  preferences → DPO update
```

## Backwards Filtering

A core validation technique: generated facets are kept only if they can reconstruct the gold standard QA pairs for each question. This ensures quality by design rather than relying solely on automated metrics.

## Evaluation Framework (Gemini-as-Judge)

Six metrics scored automatically by Gemini 2.5 Flash:

| Metric | What it measures |
|--------|-----------------|
| Taxonomy Accuracy | Correct ambiguity category assignment |
| Facet Coverage | All interpretations represented |
| Facet-Taxonomy Alignment | Consistency between facets and categories |
| Ambiguity Detection | Identifies which questions are ambiguous |
| QA Alignment Quality | Facets explain gold disambiguation answers |
| Double Validation | Quality checked in both preparation and DPO stages |

## Tech Stack

`Qwen 2.5 7B` · `Gemini 2.5 Flash API` · `HuggingFace Transformers` · `TRL (DPO)` · `Python` · `Pandas`

## Dataset

~1,000 samples containing: questions · taxonomy labels · facets · disambiguation questions · QA pairs.

> Full code + datasets: [Google Drive](https://drive.google.com/drive/u/0/folders/1oYp5PF9D24aeYMnBi1Fou9l66Bsn4MH0)

## Context

Research project for **CS 546 — Machine Learning in NLP**, University of Illinois Urbana-Champaign.
