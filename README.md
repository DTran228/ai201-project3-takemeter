"# ai201-project3-takemeter" 
"# ai201-project3-takemeter" 

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
