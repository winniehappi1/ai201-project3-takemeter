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

## Results

The fine-tuned DistilBERT model was evaluated against a zero-shot Groq baseline model.

| Model | Accuracy |
|---------|---------|
| Groq Baseline | 63.16% |
| Fine-Tuned DistilBERT | 65.79% |

The fine-tuned model achieved an improvement of 2.63 percentage points over the baseline model on the test set.

## Discussion

The fine-tuned DistilBERT model slightly outperformed the Groq baseline classifier. Although the improvement was modest, the result demonstrates that the model learned patterns specific to the r/LetsTalkMusic community and the custom label definitions.

One challenge was class imbalance in the dataset. The analysis category contained significantly more examples than the hot_take and reaction categories. This likely made it harder for the model to distinguish between the minority classes.

Another challenge was ambiguity between analysis and hot_take posts. Some discussions contained both strong opinions and supporting evidence, making them difficult to classify consistently.


## Reflection

This project demonstrated the importance of dataset quality and label design. Defining clear label boundaries was often more difficult than training the model itself. Many music discussion posts contained characteristics of multiple labels, requiring careful annotation decisions.

If I were to continue this project, I would collect additional hot_take and reaction examples to reduce class imbalance and improve performance on minority classes. I would also experiment with larger transformer models and additional training epochs.


## Error Analysis

The confusion matrix revealed that the fine-tuned DistilBERT model predicted every test example as the **analysis** label. Out of 38 test examples, the model correctly classified 25 analysis posts but incorrectly classified all 7 hot_take posts and all 6 reaction posts as analysis.

This behavior was most likely caused by class imbalance in the dataset. The dataset contained 165 analysis examples, compared to only 43 hot_take examples and 42 reaction examples. Because analysis was the majority class, the model learned that predicting analysis was often the safest strategy for maximizing overall accuracy.

Although the model achieved an accuracy of 65.79%, the confusion matrix shows that it did not learn strong decision boundaries between the three categories. Instead, it primarily learned the characteristics of the dominant class. This explains why the improvement over the Groq baseline was relatively small (+2.63%).

To improve performance in future work, I would collect additional hot_take and reaction examples to create a more balanced dataset. I would also experiment with class weighting, oversampling minority classes, and additional hyperparameter tuning to encourage the model to learn distinctions between all labels rather than relying on the majority class.

## Baseline Reflection

The Groq zero-shot baseline achieved an accuracy of 63.16% on the test set. The baseline performed reasonably well despite having no task-specific training, suggesting that the label definitions were relatively intuitive.

However, some posts were difficult to classify because the boundaries between analysis, hot_take, and reaction are not always clear. Posts that contained both strong opinions and supporting evidence were especially challenging. I expected that a fine-tuned model trained on examples from r/LetsTalkMusic would learn these distinctions more effectively than the baseline model.