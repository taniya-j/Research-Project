# Multimodal Phishing Detection for AI-Generated Phishing Attacks

## Overview
A three-class email classifier distinguishing:
- Class 0 — Legitimate emails
- Class 1 — Human-written phishing emails
- Class 2 — AI-generated phishing emails

The system combines 63 stylometric features with a fine-tunedModernBERT transformer, fused by an XGBoost classifier.

## System Architecture
Email Input
     |
Preprocessing
     |              |
Stylometric     LLM Module
Module          ModernBERT
(63 features)   (3 scores)
     |              |
  XGBoost Fusion Classifier
           |
Legitimate / Human Phishing / AI Phishing

## Key Results
| System              | Internal Macro F1 | BEC Detection |
|---------------------|------------------|---------------|
| Zero-shot Qwen2.5   | 0.5121           | 1.4%          |
| Stylometric only    | 0.9600           | 12.0%         |
| Original Fusion     | 0.9900           | 2.7%          |
| Diverse Fusion      | 0.9879           | 99.8%         |

## Key Finding
High internal accuracy (99%) can be misleading. Systems trained on a single AI source fail on phishing from different AI models. Training data diversity is the critical factor for cross-source generalisation.

## Setup Instructions

### Step 1 - Create Environment
conda create -n phishing_detection python=3.10 -y
conda activate phishing_detection

### Step 2 - Install Dependencies
pip install -r requirements.txt
python -m spacy download en_core_web_sm

### Step 3 - Install Ollama
Download from https://ollama.com
Then pull the model:
ollama pull qwen2.5:3b

### Step 4 - Configure Kaggle API
Place kaggle.json in ~/.kaggle/
Get your API key from: https://kaggle.com/settings

### Step 5 - Start Ollama
ollama serve

## Notebook Order
| Notebook                      | Description                          |
|-------------------------------|--------------------------------------|
| 01_data_prep                  | Dataset collection and preprocessing |
| 02_stylometric                | 63 stylometric feature extraction    |
| 03_ai_phishing_generation     | AI phishing generation (overnight)   |
| 04_dataset_finalisation       | Dataset balancing and finalisation   |
| 05_llm_batch_processing       | Zero-shot LLM batch processing       |
| 05b_modernbert_finetuning     | ModernBERT fine-tuning (Google Colab)|
| 06_fusion_classifier          | XGBoost fusion classifier            |
| 09_data_cleaning_retraining   | Data cleaning and diverse retraining |

## Dataset
See data/README.md for download instructions.
Raw data not included due to size and licensing constraints.

## Hardware Used
Local: Intel i5 12th Gen, NVIDIA RTX 2050 4GB, 16GB RAM
Cloud: Google Colab T4 GPU for transformer fine-tuning

## Key Parameters
ModernBERT: learning_rate=2e-5, epochs=4, batch_size=16
XGBoost: n_estimators=300, max_depth=6, learning_rate=0.1



