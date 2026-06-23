## Baseline Results

I ran a zero-shot baseline using Groq's `llama-3.3-70b-versatile`. The model was given my three label definitions and one example for each label. It was instructed to return only one valid label: `analysis`, `hot_take`, or `reaction`.

The baseline achieved an overall accuracy of 0.613 on the test set, with 31 out of 31 responses successfully parsed.

| Label    | Precision | Recall |   F1 | Support |
| -------- | --------: | -----: | ---: | ------: |
| analysis |      0.89 |   0.67 | 0.76 |      12 |
| hot_take |      0.57 |   0.36 | 0.44 |      11 |
| reaction |      0.47 |   0.88 | 0.61 |       8 |

The baseline performed best on `analysis`, especially in precision. When it predicted `analysis`, it was usually correct. However, it struggled most with `hot_take`, which had the lowest recall and F1 score. This suggests that the zero-shot model often failed to recognize bold unsupported claims, likely confusing them with either `analysis` when they included some evidence or `reaction` when they used emotional language.

The `reaction` label had high recall but lower precision, which suggests that the model was good at catching reaction-style comments but may have over-predicted this label for comments that were actually `hot_take`.

## Fine-Tuning Approach

I fine-tuned `distilbert-base-uncased` using the starter Google Colab notebook. The dataset was split automatically into train, validation, and test sets using a 70/15/15 split.

I used the default hyperparameters from the notebook: 3 training epochs, a learning rate of 2e-5, and a batch size of 16. I kept these settings because the dataset was relatively small, and I did not want to overfit too aggressively before seeing the first evaluation result.

## Fine-Tuned Model Results

The fine-tuned DistilBERT model achieved an accuracy of 0.387 on the test set.

| Model                   | Accuracy |
| ----------------------- | -------: |
| Zero-shot Groq baseline |    0.613 |
| Fine-tuned DistilBERT   |    0.387 |

The fine-tuned model performed worse than the zero-shot baseline by 0.226 accuracy points.

## Fine-Tuned Confusion Matrix

Rows are true labels. Columns are predicted labels.

| True \ Predicted | analysis | hot_take | reaction |
| ---------------- | -------: | -------: | -------: |
| analysis         |       12 |        0 |        0 |
| hot_take         |       11 |        0 |        0 |
| reaction         |        7 |        1 |        0 |

## Initial Analysis

The fine-tuned model mostly collapsed into predicting `analysis`. It correctly classified all 12 true `analysis` examples, but it failed to correctly classify any `hot_take` or `reaction` examples. It predicted `analysis` for 30 out of 31 test examples.

This suggests that the model did not actually learn the boundaries between the three labels. Instead, it may have learned a shallow pattern where many NBA discussion comments look like arguments or explanations, so it defaulted to `analysis`. This is especially clear because the model never predicted `reaction` correctly and never predicted `hot_take` correctly.

The zero-shot Groq baseline performed better because it could follow the label definitions more flexibly. DistilBERT had much less data to learn from, and the distinction between `analysis`, `hot_take`, and `reaction` is subjective. With only around 200 examples, the model may not have seen enough diverse examples of sarcastic hot takes and emotional reactions to learn those classes well.

This result shows that fine-tuning did not automatically improve performance. For this task, the quality and diversity of the labeled dataset mattered more than simply running a training pipeline.
