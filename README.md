# NLP - Sentiment Analysis on Movie Reviews: Investigating the Impact of Pre-Training Improvements with DistilBERT
**Machine Learning for Natural Language Processing**  
**ENSAE Paris — 2024–2025**  
**Authors : Paul Dupire, Floriane Zanella**

---

## Objectives 
This work investigates the **performance of a DistilBERT model for a binary sentiment classification task on movie reviews** from the ACL IMDb dataset : 
- We first establish a performance baseline by fine-tuning DistilBERT on raw reviews, then use LIME to identify the model’s dependence on high-valence sentiment words.
- To address this, we introduce a mitigation strategy that replaces extreme sentiment terms with more neutral alternatives, aiming to improve generalization—particularly for negative reviews.
- Additionally, we implement a custom tokenization function to reduce the performance loss caused by automated truncation of long inputs to the 510 first tokens using the DistilBERT tokenizer..
- Finally, we evaluate a hybrid model that combines both techniques and benchmark all approaches against the baseline and the original results from Maas et al. (2011), using metrics such as accuracy, ROC-AUC, recall, and qualitative analysis.

---

## Data 

The dataset employed in this study is the Large Movie Review Dataset released by Maas et al. (2011), commonly referred to as the ACL IMDb dataset. It consists of 50,000 movie reviews collected from the Internet Movie Database (IMDb), evenly divided into 25,000 positive and 25,000 negative examples. 

The dataset is further split into a training set and a test set, each containing 25,000 reviews,
with no overlap between them. Each review is accompanied by a binary sentiment label. Positive reviews are defined as those with original IMDb ratings of 7 or above, and negative reviews are those with ratings of 4 or below. Neutral reviews (ratings between 5 and 6) are excluded to ensure clear sentiment polarity. The reviews vary in length and writing style, containing both colloquial and formal expressions.

We further split the train dataset into full train set (22,500 reviews) and validation set (2,500 reviews) ensuring the same polarity equilibrium, to help us through model fine-tuning.

The data used can be found [here](http://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz).

Statistical examination of the dataset revealed an average review length
of approximately 230 words (standard deviation: 170 words), 7.6% of them exceeding 500 words.
The vocabulary distribution followed a small number of frequent function words and a long tail
of infrequent terms.

The notebook containing the preliminary exploration of the data, the descriptive statistics, and the suiting of the datasets to our needs is [1_Format_explore_data.ipynb](https://github.com/florianezanella/nlp_project_sentiment_analysis/blob/main/1_Format_explore_data.ipynb). 

The exploration of the data was mostly equally shared between the two authors, with the statitistics regarding the reviews' distribution across movies and grades done by Floriane Zanella and the distribution of review length and word frequency by Paul Dupire.

---

## Methodology
The project was conducted by the two authors in equal ways and each part of the project has been under discussion and co-construction with the two authors working with no exceptions. For the requirements of the course, the participation of each authors is outlined when it was greater than the other. It is although important to keep in mind that each author contributed to every part. 

The notebook containing the code used following this methodology as well as the results is [2_Model_training_and_evaluation.ipynb](https://github.com/florianezanella/nlp_project_sentiment_analysis/blob/main/2_Model_training_and_evaluation.ipynb). 


**A. Baseline model and classification** - Floriane Zanella and Paul Dupire
1. **Fine-tune the baseline model**
2. **Pre-process using the DistilBERT tokenizer**, applying WordPiece segmentation, lowercasing the text, and truncating reviews to 512 tokens, with padding for shorter reviews
3. **Performance evaluation**
   
**B. LIME analysis of the baseline model and sentiment mitigation** - Paul Dupire (mostly)
1. **Interpretation with LIME** :  
  - Analyze the internal decision-making of the model by **perturbing input text and observing the impact on predictions**
  - Fit a sparse linear model to **approximate token influences**
  - Conduct analysis on **six misclassified test set examples**: three with the highest confidence incorrect predictions and three randomly sampled from the misclassified set for comparison.
2. **Sentiment mitigation**
  - **Replace sentiment-laden words in reviews** with more neutral alternatives to reduce bias.
  - Identify key influential tokens contributing to misclassifications, especially in negative reviews.
  - Explore the impact of sentiment mitigation on improving generalization
3. **Performance Evaluation**
  - Compare the baseline and mitigated models to assess improvements in classification balance.

**C. Addressing tokenization truncation** - Floriane Zanella (mostly)
1. **Implement custom tokenization** to handle long reviews with nuanced sentiment shifts by truncating to the first 128 and last 382 tokens, preserving both the beginning and end of the review
2. **Re-evaluate the DistilBERT model using the custom tokenization** (Custom Tokenization Model)
3. **Combine it with lexical sentiment mitigation** (Mixed Model)
4. **Performance Evaluation**
  - Compare the baseline and mitigated models to assess improvements in classification balance.



---

## Key results
The baseline DistilBERT model achieves the following performance metrics:

- **Accuracy**: 93.14%
- **Precision**: 92.96%
- **Recall**: 93.34%
- **F1 score**: 93.15%
- **ROC-AUC**: 0.98

The confusion matrix shows a balanced performance, with slightly more false positives for positive reviews.

### Mitigated Model
The mitigated model, where sentiment-laden words are replaced with neutral alternatives, achieves:

- **Accuracy**: 92.85%
- **Precision**: 93.58%
- **Recall**: 92.02%
- **F1 score**: 92.79%

This model reduces false positives (883 to 789) but increases false negatives (832 to 998), improving negative class performance at the expense of positive class sensitivity.

### Custom Tokenization Model
The custom tokenization model, which keeps the first 128 and last 382 tokens of long reviews, achieves:

- **Accuracy**: 93.75%
- **Precision**: 93.48%
- **Recall**: 94.06%
- **F1 score**: 93.77%

This model improves recall and precision, addressing truncation issues and preserving contextual information in longer reviews.

### Mixed Model
The mixed model, combining lexical mitigation and custom tokenization, achieves:

- **Accuracy**: 93.42%
- **Precision**: 92.29%
- **Recall**: 94.76%
- **F1 score**: 93.51%

It offers the best recall but does not surpass the custom tokenization model in overall accuracy.

### Table 1: Classification Metrics for the Different DistilBERT Models on the IMDb Test Set

| Model                             | Accuracy | Precision | Recall  | F1 Score |
|-----------------------------------|----------|-----------|---------|----------|
| Maas et al.                       | 88.89%   | -         | -       | -        |
| Baseline                          | 93.15%   | 92.65%    | 93.74%  | 93.19%   |
| Mitigated                         | 92.85%   | 93.58%    | 92.02%  | 92.79%   |
| Custom Tokenization               | 93.75%   | 93.48%    | 94.06%  | 93.77%   |
| Mixed (Mitigated + Custom Token.) | 93.42%   | 92.29%    | 94.76%  | 93.51%   |


## Conclusion

In summary, the baseline DistilBERT model performs strongly with an accuracy of 93.14% and a ROC-AUC score of 0.98. The mitigated model, which replaces sentiment-laden words with neutral alternatives, slightly reduces overall accuracy but improves performance on negative reviews by lowering false positives. The custom tokenization model, which addresses truncation issues, shows an improvement in recall and precision, demonstrating the value of preserving more contextual information in long reviews. Finally, the mixed model, combining both lexical mitigation and custom tokenization, provides the best recall but does not surpass the custom tokenization model in overall accuracy.

These results highlight the importance of addressing both lexical biases and tokenization limitations when pretraining models to enhance the performance of sentiment classification models. These performances significantly surpass those of Maas et al. (2011), which was our latent goal.

---
## Notebooks, report and data
The data can be found [here](http://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz).

The notebook containing the preliminary exploration of the data, the descriptive statistics, and the suiting of the datasets to our needs is [1_Format_explore_data.ipynb](https://github.com/florianezanella/nlp_project_sentiment_analysis/blob/main/1_Format_explore_data.ipynb). 

The notebook containing the code used following this methodology as well as the results is [2_Model_training_and_evaluation.ipynb](https://github.com/florianezanella/nlp_project_sentiment_analysis/blob/main/2_Model_training_and_evaluation.ipynb). 

The report can be found [here](https://github.com/florianezanella/nlp_project_sentiment_analysis/blob/main/NLP_mini_project_report___Paul_Dupire__Floriane_Zanella.pdf).
