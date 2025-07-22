# Garchen-rinpoche-speech-sig
AI-powered transcription and translation of Garchen Rinpoche's teachings
# Discovery Phase Report: Development of Speech-to-Text for Garchen Rinpoche Teachings

## Introduction

This report summarizes the discovery phase for developing a specialized Speech-to-Text (STT) solution for Garchen Rinpoche's Tibetan teachings. The project aims to improve transcription accuracy and efficiency for preserving these valuable Buddhist teachings. Our discovery process was conducted in multiple phases, systematically evaluating existing resources, testing various models, and measuring the impact of customization efforts.

## Phase 1: Audio Resource Cataloging

We began by creating a comprehensive catalog of all available Garchen Rinpoche audio recordings with detailed metadata. This catalog provided crucial insights into:

- Total number of audio files available
- Cumulative duration of teachings
- Audio quality and recording environments
- Content categorization and metadata structure

**Resource:** [Complete Audio Catalog (Google Sheets)](https://docs.google.com/spreadsheets/d/1P60PS45yMAHOQZzxw8WM8IBrVbOL6DCeHyVC6Pv6oFE/edit?pli=1&gid=337766163#gid=337766163)

This cataloging effort established the foundation for all subsequent work, allowing us to understand the scope and characteristics of the audio data we would be working with.

## Phase 2: Model Evaluation on Garchen Rinpoche Audio

In this critical phase, we evaluated multiple existing STT models using a test set of segmented Garchen Rinpoche audio recordings. The evaluation compared model-generated transcripts against human-produced reference transcripts using industry-standard metrics:

- Character Error Rate (CER)
- Word Error Rate (WER)
- Syllable Error Rate (SER)

### Key Results

| Model | CER | WER | SER | Major Error Type |
|-------|-----|-----|-----|------------------|
| General STT | 27.53% | 57.24% | 51.15% | Substitutions (75%) |
| Situ Rinpoche | 28.92% | 60.68% | 53.68% | Substitutions (75%) |
| Dilgo Khyentse | 65.96% | 92.63% | 91.55% | Deletions (33%) |

Our evaluation determined that the **General STT model** performed best across all metrics, contrary to the initial hypothesis that models trained on similar religious speech would outperform a general-purpose model.

**Resources:**
- [Evaluation Dataset (HuggingFace)](https://huggingface.co/datasets/ganga4364/garchen_rinpoche_inference_transcripts)
- [Detailed Evaluation Report](https://forum.openpecha.org/t/stt-evaluation-report-on-garchen-rinpoche-by-different-models/323)

Based on these findings, we selected the General STT model as our baseline for generating initial transcripts, which could then be edited by human transcribers.

## Phase 3: Data Collection and Benchmark Creation

Following model selection, we focused on:

1. **Tracking transcribed data** until we accumulated 5 hours of training data
2. **Creating a benchmark dataset** with audio samples not included in the training data

This phase was critical for establishing both:
- A dataset for fine-tuning the selected base model
- A standardized benchmark for objective evaluation of model improvements

**Resources:**
- [Transcription Progress Tracker](https://stt.pecha.tools)
- [Report Generator Repository](https://github.com/OpenPecha/stt-report-generator)
- [Benchmark Dataset](https://huggingface.co/datasets/ganga4364/garchen_rinpoche_benchmark)

## Phase 4: Model Fine-tuning for Garchen Rinpoche

Upon collecting sufficient training data (5+ hours), we fine-tuned the base model specifically for Garchen Rinpoche's unique speech patterns. The fine-tuning process involved:

- Base Model: `ganga4364/mms_300_v4.96000`
- Architecture: `Wav2Vec2ForCTC`
- Training Dataset: 5:33:00 hours (3,071 samples)
- Test Dataset: 1:04:12 hours (893 samples)

### Fine-tuning Results

| Metric | Base Model | Fine-Tuned Model | Improvement |
|--------|------------|------------------|-------------|
| CER | 27.67% | 22.93% | 4.74% |
| WER | 45.92% | 39.42% | 6.50% |

The fine-tuning process yielded significant improvements across all error metrics, with training checkpoints showing steady progress:

- 5,000 steps: 27.41% CER
- 10,000 steps: 23.37% CER
- 19,000 steps: 22.93% CER (final)

**Resources:**
- [Fine-tuning Documentation](https://forum.openpecha.org/t/custom-speech-to-text-model-for-garchen-rinpoche-s-teachings/331)
- [Benchmark Results](https://huggingface.co/datasets/ganga4364/garchen_rinpoche_benchmark_result)

## Phase 5: Transcription Speed Analysis

The final phase examined whether the effort invested in fine-tuning was justified by measurable improvements in transcription speed. We conducted a comparative study with four experienced transcribers using three different approaches:

1. Manual transcription (no assistance)
2. Base model-assisted transcription
3. Fine-tuned model-assisted transcription

### Speed Analysis Results

| Metric | Manual | Base Model | Fine-tuned Model |
|--------|--------|------------|------------------|
| Average Speed (chars/min) | 47.86 | 49.85 | 77.67 |
| Time Saved vs Manual | - | 3.52 min | 40.60 min |
| Speed Improvement | - | 3.31% | 38.25% |

#### Key Findings

- **Base Model Paradox:** Surprisingly, the base model assistance sometimes slowed down transcription (3 of 4 transcribers were slower)
- **Fine-tuned Model Success:** All transcribers showed significant speed improvements with the fine-tuned model (12-47% improvement)
- **Individual Variations:** Transcribers showed different adaptation patterns to the assisted workflows

**Resource:** [Transcription Speed Analysis](https://forum.openpecha.org/t/transcription-speed-analysis-garchen-rinpoche/405)

## Conclusions

Our discovery phase yielded several important insights:

1. **Model Selection:** The general STT model outperformed specialized models for Garchen Rinpoche audio
2. **Fine-tuning Effectiveness:** Speaker-specific fine-tuning yielded significant error rate reductions (4.74% CER improvement)
3. **Practical Impact:** Fine-tuned model assistance increased transcription speed by 38.25% on average
4. **Unexpected Finding:** Base model assistance sometimes decreased transcription speed, revealing the importance of high-accuracy transcripts

## Interactive Testing

To make our fine-tuned model accessible for testing, we've created a Hugging Face Space that allows users to:
- Upload Garchen Rinpoche audio files
- Generate transcripts using our fine-tuned model
- Download SRT subtitle files for video integration

**Resource:** 
- [Garchen Rinpoche Audio Subtitle Generator Space](https://huggingface.co/spaces/openpecha/Garchen_Rinpoche_STT)
- [General Audio Subtitle Generator Space](https://huggingface.co/spaces/openpecha/stt_demo)
## Next Steps

Based on these findings, we recommend:

1. Continue using the fine-tuned model for all Garchen Rinpoche transcription projects
2. Expand the training dataset to further improve model accuracy
3. Develop specialized transcriber training for optimal use of STT-assisted workflows
4. Investigate why base models sometimes reduce efficiency and develop mitigation strategies
5. Promote the interactive testing space to gather more user feedback

---

*This discovery phase report was prepared by the OpenPecha team as part of our ongoing efforts to preserve and make accessible important Buddhist teachings through modern language technologies.*
