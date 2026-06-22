# TakeMeter: Music Discussion Classification

## Project Overview

TakeMeter is a machine learning project that classifies music discussion posts from r/LetsTalkMusic into three discourse categories:

* analysis
* hot_take
* reaction

The goal is to determine whether a post is presenting a reasoned argument, expressing a bold opinion, or sharing an emotional reaction to music-related content.

---

# Community Selection

## Community

**r/LetsTalkMusic**

r/LetsTalkMusic is a discussion-focused music community where users share opinions about artists, albums, genres, music history, and industry trends.

I selected this community because it contains a wide variety of discourse styles. Some users write detailed arguments supported by evidence, others share strong opinions with little justification, and many posts express emotional reactions to music experiences. This diversity makes the community well suited for a text classification task.

---

# Label Taxonomy

## analysis

A post that supports an opinion using reasoning, evidence, examples, comparisons, or detailed explanation.

### Examples

1. "Kendrick Lamar's albums stand out because they combine storytelling with social commentary and recurring themes."

2. "The Beatles influenced modern pop music through innovations in songwriting and studio recording techniques."

---

## hot_take

A strong opinion expressed confidently with little or no supporting evidence.

### Examples

1. "Taylor Swift is the most overrated artist of all time."

2. "Rock music died years ago."

---

## reaction

An emotional response to a song, album, artist, concert, or music-related event.

### Examples

1. "I just listened to this album and it completely blew my mind."

2. "This song is amazing. I can't stop replaying it."

---

# Dataset Collection

## Data Source

All examples were collected from public posts and comments on r/LetsTalkMusic.

## Dataset Size

Total Examples: **250**

| Label    | Count |
| -------- | ----: |
| analysis |   165 |
| hot_take |    43 |
| reaction |    42 |

## Annotation Process

Each example was manually reviewed and assigned a label according to the definitions established in planning.md.

Although I originally intended to create a more balanced dataset, the community naturally contains many more analytical discussions than emotional reactions or unsupported opinions.

---

# Difficult Annotation Cases

## Case 1

**Post:**

"Kanye West is the greatest producer ever because his influence can be heard across hip-hop, pop, and electronic music."

**Possible Labels**

* analysis
* hot_take

**Decision**

If the post develops reasoning and provides evidence, it is labeled as analysis. If it mainly makes a bold claim without support, it is labeled as hot_take.

---

## Case 2

**Post:**

"Taylor Swift's success comes more from marketing than musical innovation."

**Possible Labels**

* hot_take
* analysis

**Decision**

If specific examples and comparisons are provided, it is labeled as analysis. Otherwise, it is labeled as hot_take.

---

## Case 3

**Post:**

"I just listened to In Rainbows and I think it completely changed modern indie music."

**Possible Labels**

* reaction
* analysis

**Decision**

If the post mainly expresses excitement or personal feelings, it is labeled as reaction. If it develops an argument about influence and evidence, it is labeled as analysis.

---

# Baseline Model

## Baseline Approach

The baseline model used Groq's Llama 3.3 70B model in a zero-shot classification setting.

The prompt included:

* Community description
* Label definitions
* One example for each label
* Instructions to return only the label name

The model classified every example in the test set and its performance was recorded for comparison against the fine-tuned model.

---

# Fine-Tuning Approach

## Model

DistilBERT (`distilbert-base-uncased`)

## Training Setup

Training was performed using the provided Google Colab notebook on a T4 GPU.

### Hyperparameters

* Epochs: 3
* Learning Rate: 2e-5
* Batch Size: 16

I used the default training settings because the dataset was relatively small and these parameters are commonly used for transformer fine-tuning tasks.

---

# Evaluation Results

## Accuracy Comparison

| Model                 | Accuracy |
| --------------------- | -------: |
| Groq Baseline         |   63.16% |
| Fine-Tuned DistilBERT |   65.79% |

The fine-tuned model improved performance by **2.63 percentage points** over the baseline.

---

## Confusion Matrix

| True Label | Predicted Analysis | Predicted Hot Take | Predicted Reaction |
| ---------- | -----------------: | -----------------: | -----------------: |
| analysis   |                 25 |                  0 |                  0 |
| hot_take   |                  7 |                  0 |                  0 |
| reaction   |                  6 |                  0 |                  0 |

The confusion matrix shows that the model predicted every test example as **analysis**.

---

# Error Analysis

## Example 1

**True Label:** hot_take

**Predicted Label:** analysis

**Text**

> Country is the fastest growing genre. Anyone else think Rock will be up next?

**Analysis**

The post expresses a prediction and opinion but provides little evidence. The model appears to associate discussions about music trends with analytical reasoning.

---

## Example 2

**True Label:** hot_take

**Predicted Label:** analysis

**Text**

> So really, why the hate for Billy Corgan? He seems respectful and well-researched on his podcast.

**Analysis**

The post contains a personal opinion rather than a structured argument. The model likely interpreted descriptive language as evidence.

---

## Example 3

**True Label:** reaction

**Predicted Label:** analysis

**Text**

> What's the weirdest reason you stopped listening to a band that you previously enjoyed?

**Analysis**

The post encourages personal experiences and emotional responses. Because it is discussion-oriented, the model incorrectly classified it as analysis.

---

# Sample Classifications

| Example                                             | True Label | Predicted Label | Confidence |
| --------------------------------------------------- | ---------- | --------------- | ---------- |
| Country is the fastest growing genre...             | hot_take   | analysis        | 0.36       |
| Why the hate for Billy Corgan?                      | hot_take   | analysis        | 0.35       |
| What's the weirdest reason you stopped listening... | reaction   | analysis        | 0.35       |
| Kendrick Lamar's albums stand out because...        | analysis   | analysis        | 0.81       |
| The Beatles influenced modern pop music...          | analysis   | analysis        | 0.79       |

### Correct Prediction Example

The Kendrick Lamar example was correctly classified as analysis because it provides reasoning and discusses specific characteristics of the artist's work rather than expressing a simple opinion or emotional reaction.

---

# Reflection

The fine-tuned DistilBERT model slightly outperformed the Groq baseline. However, the confusion matrix revealed that the model learned to predict the majority class almost exclusively.

This suggests that the model learned the general characteristics of analytical music discussions but struggled to distinguish hot_take and reaction posts. The primary cause appears to be dataset imbalance.

If I continued this project, I would collect additional hot_take and reaction examples to create a more balanced dataset. I would also experiment with class weighting, oversampling techniques, and additional hyperparameter tuning.

---

# Spec Reflection

The planning document helped establish clear label definitions before data collection began. Having explicit decision rules made annotation more consistent and reduced ambiguity.

However, implementation diverged from the original plan because the final dataset became heavily imbalanced. I initially intended to collect a more balanced number of examples per label, but r/LetsTalkMusic naturally contained many more analysis posts than the other categories.

---

# AI Usage

## Example 1

I used ChatGPT to review and refine my label definitions before collecting data. The goal was to make the distinctions between analysis, hot_take, and reaction more precise.

## Example 2

I used ChatGPT to analyze the confusion matrix and identify possible causes of model errors. The AI suggested that class imbalance was contributing to the model's tendency to predict the majority class. I verified this conclusion manually by reviewing the label distribution and confusion matrix.

All final annotation decisions and conclusions were reviewed manually.

---

# Repository Structure

```text
ai201-project3-takemeter/
├── planning.md
├── README.md
├── dataset/
│   └── labeled_music_posts.csv
├── evaluation/
│   ├── evaluation_results.json
│   └── confusion_matrix.png
└── notebooks/
    └── ai201_project3_takemeter.ipynb
```
# Demo Video

A walkthrough of the TakeMeter project, including the dataset, label definitions, model training process, evaluation results, and error analysis.

Loom Recording:
https://www.loom.com/share/f68dc9ff42e54794bd5337f4e74b03ed