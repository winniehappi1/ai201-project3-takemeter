# TakeMeter Planning

## 1. Community

I chose **r/LetsTalkMusic** because it is a discussion-focused music community where users share opinions about artists, albums, genres, and trends in music. Posts range from emotional reactions to detailed arguments supported by examples and comparisons, making it a strong community for studying discourse quality.

The community is a good fit for a classification task because it contains a variety of discourse styles. Some users provide evidence-based arguments, others share strong opinions with little support, and others simply react emotionally to music-related experiences. These differences make it possible to define meaningful and distinguishable labels.

---

## 2. Labels

### Analysis

A post that supports an opinion using reasoning, examples, comparisons, or evidence.

**Example 1:**

> "Kendrick Lamar's albums stand out because they combine storytelling with social commentary and recurring themes."

**Example 2:**

> "The Beatles influenced modern pop music through innovations in songwriting and studio recording techniques."

### Hot Take

A strong opinion expressed confidently with little or no supporting evidence.

**Example 1:**

> "Taylor Swift is the most overrated artist of all time."

**Example 2:**

> "Rock music died years ago."

### Reaction

An emotional response to a song, album, artist, concert, or music-related event.

**Example 1:**

> "I just listened to this album and it completely blew my mind."

**Example 2:**

> "This song is amazing. I can't stop replaying it."

---

## 3. Hard Edge Cases

### Difficult Case 1

**Post:**

> "Kanye West is the greatest producer ever because his influence can be heard across hip-hop, pop, and electronic music."

**Possible Labels:**

* Analysis
* Hot Take

**Decision Rule:**
If the author develops the reasoning and explains the evidence, label it as **analysis**. If the post mainly makes a bold claim without developing an argument, label it as **hot_take**.

### Difficult Case 2

**Post:**

> "Taylor Swift's success comes more from marketing than musical innovation."

**Possible Labels:**

* Analysis
* Hot Take

**Decision Rule:**
If the post explains specific examples of marketing strategies and compares them to musical contributions, label it as **analysis**. Otherwise, label it as **hot_take**.

### Difficult Case 3

**Post:**

> "I just listened to In Rainbows and I think it completely changed modern indie music."

**Possible Labels:**

* Reaction
* Analysis

**Decision Rule:**
If the post mainly expresses excitement or personal feelings, label it as **reaction**. If it develops an argument about influence and provides evidence, label it as **analysis**.

---

## 4. Data Collection Plan

Examples will be collected from public posts and comments in **r/LetsTalkMusic**. The goal is to collect at least 200 labeled examples representing the three categories: analysis, hot_take, and reaction.

Initially, I aimed for a balanced dataset across all labels. However, because r/LetsTalkMusic naturally contains more analytical discussions than emotional reactions or unsupported opinions, the final dataset contains more analysis examples. If a label becomes underrepresented, I will collect additional examples from discussion threads that specifically contain that type of discourse.

The final dataset contains 250 labeled examples.

---

## 5. Evaluation Metrics

The primary evaluation metric will be **accuracy** because this is a multi-class classification problem. However, accuracy alone is not sufficient because the dataset is not perfectly balanced.

Additional evaluation methods include:

* Confusion Matrix
* Per-class performance analysis
* Comparison against a zero-shot Groq baseline model

The confusion matrix is especially important because it shows which labels are being confused with one another and helps identify systematic classification errors.

---

## 6. Definition of Success

A successful classifier should achieve performance that exceeds the zero-shot Groq baseline model and correctly distinguish between the three discourse categories.

For this project, I consider a model successful if it:

* Achieves at least 60% accuracy on the test set.
* Outperforms the Groq baseline model.
* Demonstrates the ability to distinguish between analysis, hot_take, and reaction posts.

In a real-world deployment, I would also require reasonable performance across all labels rather than strong performance on only the majority class.

---

## 7. AI Tool Plan

### Label Stress-Testing

I used ChatGPT to review and refine my label definitions before collecting data. The goal was to ensure that the boundaries between analysis, hot_take, and reaction were clear and mutually exclusive.

### Annotation Assistance

I used AI assistance to review example posts and verify that the label definitions were being applied consistently. Final labeling decisions were reviewed manually before inclusion in the dataset.

### Failure Analysis

After training, I used AI assistance to interpret the confusion matrix and identify patterns in the model's errors. Any conclusions drawn from AI-generated analysis were verified against the actual evaluation results.
