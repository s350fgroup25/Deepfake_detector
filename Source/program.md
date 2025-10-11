## /program 
## dataset_sentence.py
**Content**: Dataset handling for ASVspoof2019 audio files with audio preprocessing and collation.
- ASVspoof2019Dataset: Loads audio files and labels from ASVspoof2019 dataset
- repeat_samples(): Pads/crops audio to fixed length (64000 samples)
- AudioCollator: Batch processing with Wav2Vec2 feature extraction

Usage: Data loading and preprocessing for anti-spoofing audio classification.

##  eer1.py
**Content:** Equal Error Rate (EER) calculation implementation.
- eer(): Computes EER and threshold from scores and ground truth labels

Usage: Performance evaluation metric for spoofing detection systems.

## eval-sentence.py
**Content:** Evaluation script for trained anti-spoofing models.
- Loads pretrained model from safetensors file
- Evaluates on ASVspoof2019 LA eval set
- Computes EER using eer1.py

Usage: Model performance evaluation and metric calculation.

## model.py
**Content:** Base model architecture with attentive pooling mechanisms.
- Self_Attentive_Pooling, SelfAttentionPooling, Attentive_Statistics_Pooling: Different pooling strategies
- Model: Wav2Vec2-based classifier with additional layers

Usage: Defines neural network architecture for audio classification.

## model_sentence1.py
**Content:** Enhanced model with SSL (Self-Supervised Learning) backbone.
- SSLModel: Wrapper for various SSL models (WavLM, HuBERT, Distil variants)
- Model: Main classification model with attentive statistics pooling

Usage: Primary model architecture training/evaluation.

## model_sentence2.py
**Content:** Alternative multi-layer aggregation model architecture.
- Aggregates features from multiple hidden layers
- More complex pooling and fusion mechanism

Usage: Experimental model variant with layer-wise feature aggregation.

## train-sentence.py
**Content:** Training script for anti-spoofing models.
- Sets up training environment and data loaders
- Configures training arguments (batch size, learning rate, etc.)
- Implements early stopping and model saving

Usage: Main training script to train audio spoofing detection models.

## Overall Usage Flow:
- Train: train-sentence.py → Trains model → Saves to .safetensors
- Evaluate: eval-sentence.py → Loads model → Computes EER
- Data: dataset_sentence.py → Handles audio preprocessing
- Metrics: eer1.py → Calculates evaluation metrics

Purpose: Audio spoofing detection system using self-supervised learning models on ASVspoof2019 dataset.
