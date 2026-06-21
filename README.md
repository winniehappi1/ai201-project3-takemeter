# TakeMeter

A machine learning project that classifies music discussion posts from r/LetsTalkMusic into different discourse categories.

## Community

r/LetsTalkMusic

This community focuses on discussions about music, artists, albums, genres, and trends. Users frequently post detailed arguments, strong opinions, and emotional reactions.

## Labels

### analysis
A post that supports an opinion using reasoning, examples, comparisons, or evidence.

### hot_take
A strong opinion expressed confidently with little or no supporting evidence.

### reaction
An emotional response to a song, album, artist, concert, or music-related event.

## Dataset

Community: r/LetsTalkMusic

Total examples: 250

Labels:
- analysis: 165
- hot_take: 43
- reaction: 42

## Model

Fine-tuned DistilBERT (`distilbert-base-uncased`).

## Repository Structure

```
ai201-project3-takemeter/
├── planning.md
├── README.md
├── dataset/
├── evaluation/
└── notebooks/
```

## Status

- [x] Community selected
- [x] Label taxonomy defined
- [x] Dataset collected
- [ ] Model trained
- [ ] Evaluation completed
- [ ] Baseline comparison completed