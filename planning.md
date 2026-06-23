# TakeMeter Planning

## Project Idea

TakeMeter is a text classifier for NBA/basketball discussion posts and comments. The goal is to classify each example into one of three labels: `analysis`, `hot_take`, or `reaction`. I want the classifier to learn the difference between structured basketball reasoning, bold unsupported opinions, and emotional reactions.

## Community

I chose NBA/basketball discussion communities, especially public Reddit-style posts and comments from r/nba. This community is a good fit for a classification task because the discourse is very active and varied. People discuss games, trades, player rankings, coaching, team building, playoff results, and highlights.

The community has a wide range of discourse quality. Some comments are detailed and analytical, some are strong opinions without much support, and some are just jokes or emotional reactions. These differences matter because basketball fans often argue about whether a comment is real analysis or just a hot take.

## Labels

### `analysis`

A post is labeled `analysis` when it makes a structured basketball argument using specific reasoning, roster logic, statistics, matchup details, historical comparison, or tactical observation.

Examples:

1. “That’s the issue though. He’s so good that they keep hovering in the mediocre zone. Not good enough to go far but not bad enough for any worthy draft picks.”

This is `analysis` because the comment explains a team-building problem. It argues that Spoelstra keeps the Heat competitive, but that also prevents them from being bad enough to get high draft picks.

2. “I mean they’re pretty clearly not keeping most assets they get and just flipping them later. They wouldn’t keep Brown from Boston long either. Once the Giannis trade is done then it’s full rebuild mode.”

This is `analysis` because the comment explains a possible trade strategy. It gives reasoning about asset flipping, Jaylen Brown, and a rebuild plan after a Giannis trade.

### `hot_take`

A post is labeled `hot_take` when it makes a bold or confident basketball opinion but gives little, weak, or incomplete supporting evidence.

Examples:

1. “They simply just can not match that offer, at all.”

This is `hot_take` because it makes a confident claim about a trade package, but it does not explain the assets, salary, picks, or team situation in detail.

2. “You’ll be in a better spot than more than half of the league as long as you have Spo.”

This is `hot_take` because it makes a strong claim about the Heat’s position compared to the rest of the league, but it mostly depends on a general belief in Spoelstra without much support.

### `reaction`

A post is labeled `reaction` when it is mainly an immediate emotional response, joke, hype, frustration, or short reaction to a basketball event.

Examples:

1. “GLORIOUS HATEWATCH.”

This is `reaction` because it is mainly an emotional response to another team losing. It does not try to make a real argument.

2. “Holy fucking shit. That’s the most clutch tip I’ve ever seen.”

This is `reaction` because it is an immediate emotional response to a highlight. It expresses excitement but does not give detailed reasoning.

## Hard Edge Cases

The hardest boundary is between `analysis` and `hot_take`. Some comments include one piece of evidence but still feel like bold unsupported opinions. For example:

“You want the Bam/Herro combo that has shown that its ceiling is the play-in with the best head coach in the NBA?”

This could be `analysis` because it refers to past team results and roster ceiling. However, the comment is mostly written as a sarcastic criticism instead of a full argument. I would label it `hot_take` because the evidence is used more like a punchline than structured reasoning.

Another hard boundary is between `analysis` and `reaction`. Some reactions include factual details, but the main purpose is still emotional. For example:

“Champagnie was undrafted out of college and he is playing like 20 minutes a game in the WCF making league minimum money. That’s fucking crazy.”

This could be `analysis` because it includes factual details about a player’s background, minutes, and salary. However, the main purpose is still to express surprise. I would label it `reaction` unless the post continues by explaining how his role affects the game or the team’s strategy.

A third edge case is between `hot_take` and `reaction`. For example:

“From a historical half-time Finals lead to an all-time choke job. This series is on crack.”

This could be `reaction` because it is emotional and dramatic, but it also makes a strong judgment about the series being an all-time choke. I would label it `hot_take` because it makes a bold claim, even though the language is emotional.

## Decision Rules

I will label each post based on its main purpose.

If the post explains a basketball idea with specific reasoning, I will label it `analysis`.

If the post makes a strong claim without enough support, I will label it `hot_take`.

If the post mainly expresses emotion, humor, hype, or frustration, I will label it `reaction`.

For posts that include both opinion and evidence, I will ask whether the evidence is part of a real explanation. If yes, the label is `analysis`. If the evidence is vague, cherry-picked, sarcastic, or only used to make the opinion sound stronger, the label is `hot_take`.

For posts that include facts but mainly express surprise or emotion, I will label them `reaction`.

## Difficult Annotation Examples Found During Labeling

During annotation, I found several examples that were difficult to label because they sat between two categories. These examples helped me clarify my decision rules.

### Difficult Example 1

Text: “Champagnie was undrafted out of college and he is playing like 20 minutes a game in the WCF making league minimum money. That’s fucking crazy.”

Possible labels: `analysis` or `reaction`

Final label: `reaction`

Reason: This comment includes factual details about the player’s background, minutes, and salary, so it could look like `analysis`. However, the main purpose of the comment is to express surprise. It does not explain how his role affects the team, the game plan, or the result. Because the main purpose is emotional reaction, I labeled it `reaction`.

### Difficult Example 2

Text: “You want the Bam/Herro combo that has shown that its ceiling is the play-in with the best head coach in the NBA?”

Possible labels: `analysis` or `hot_take`

Final label: `hot_take`

Reason: This comment uses a real basketball point: the Bam/Herro pairing did not lead to a high playoff ceiling, even with a strong coach. However, the comment is mostly written as a sarcastic criticism, not a structured argument. The evidence is used more like a punchline than a full explanation. Because of that, I labeled it `hot_take`.

### Difficult Example 3

Text: “From a historical half-time Finals lead to an all-time choke job. This series is on crack.”

Possible labels: `hot_take` or `reaction`

Final label: `hot_take`

Reason: This comment is emotional and dramatic, so it could be considered `reaction`. However, it also makes a strong judgment by calling the series an “all-time choke job.” Since it is making a bold claim without detailed support, I labeled it `hot_take`.

### Difficult Example 4

Text: “Shead tripped on Poeltl's foot lmao, not really an ankle breaker from Harden.”

Possible labels: `reaction` or `analysis`

Final label: `analysis`

Reason: The “lmao” makes the comment feel casual and reactive, but the comment is actually explaining what happened in the play. It gives a specific reason why the highlight should not be considered a true ankle breaker. Because it explains the event rather than only reacting to it, I labeled it `analysis`.

### Difficult Example 5

Text: “This is so ridiculous man. It's just basketball bro.”

Possible labels: `reaction` or `hot_take`

Final label: `reaction`

Reason: This comment makes a judgment about the situation, but it does not really make a larger argument. Its main purpose is to express disbelief and frustration. Because the comment is mostly emotional, I labeled it `reaction`.

## Label Distribution After Annotation

After collecting and labeling the dataset, the final label distribution was:

| Label    | Count | Percentage |
| -------- | ----: | ---------: |
| analysis |    83 |      40.1% |
| hot_take |    67 |      32.4% |
| reaction |    57 |      27.5% |
| Total    |   207 |       100% |

No single label accounts for more than 70% of the dataset. The dataset is not perfectly balanced, but each label has enough examples for the model to learn from. `analysis` is the largest class, but `hot_take` and `reaction` are still well represented.


## Data Collection Plan

I will collect at least 200 public NBA/basketball posts or comments. I will use public posts only, mainly from r/nba discussion threads, trade discussion posts, post-game threads, highlight threads, and more serious basketball discussion threads.

I will save the dataset as one CSV file with three columns:

* `text`
* `label`
* `notes`

I will not split the dataset manually. The notebook will split it into train, validation, and test sets automatically.

My target label distribution is:

| Label    | Target Count |
| -------- | -----------: |
| analysis |        65-75 |
| hot_take |        65-75 |
| reaction |        65-75 |

I will try to keep the dataset balanced so that no single label is more than 70% of the dataset. If one label is underrepresented, I will collect more examples for that label before training.

To avoid topic bias, I will not collect all examples from one thread or one topic. I will mix examples from different types of NBA discussions, such as:

* trade rumors
* playoff reactions
* highlight threads
* draft discussions
* player debates
* team-building discussions
* serious basketball analysis threads

This should help the model learn discourse type instead of memorizing one topic, team, or player.

## Evaluation Metrics

I will use overall accuracy to measure how often the model predicts the correct label. However, accuracy alone is not enough because the labels may not be perfectly balanced. A model could get decent accuracy by doing well on common labels but still fail on a harder label.

I will also use per-class precision, recall, and F1 score. These metrics are important because I want to know whether the model handles each label well.

Precision will show whether the model is over-predicting a label. Recall will show whether the model is missing true examples of a label. F1 score will help summarize the balance between precision and recall for each class.

I will also use a confusion matrix to see which labels are being confused. This is especially important for this project because the most important boundary is probably between `analysis` and `hot_take`.

## Definition of Success

A successful classifier should perform better than the zero-shot Groq baseline on the same test set. I would consider the fine-tuned model successful if it reaches at least 70% overall accuracy and has no label with an F1 score below 0.60.

For a real community tool, I would want higher performance and more data. However, for this project, I would consider the model useful if it clearly beats the baseline and if the remaining errors are understandable. If the model struggles, I will focus on explaining why it failed and what that says about the label definitions or dataset.

## AI Tool Plan

### Label Stress-Testing

I will use an AI tool to stress-test my label definitions before annotating the full dataset. I will give the AI my definitions for `analysis`, `hot_take`, and `reaction`, then ask it to generate borderline examples between the labels. If I cannot classify those examples clearly, I will revise my definitions or decision rules before labeling all 200 examples.

### Annotation Assistance

I may use an LLM to pre-label some examples, but I will manually review every label myself. I will not accept AI labels automatically. If the AI gives a label that does not match my definitions, I will correct it. If I use AI pre-labeling, I will disclose it in the README.

### Failure Analysis

After training and evaluation, I will use an AI tool to help identify patterns in the model’s wrong predictions. I will ask it to look for common themes, such as short comments, sarcastic comments, emotional language, or confusion between `analysis` and `hot_take`. I will verify the patterns myself by rereading the misclassified examples before including them in the final report.

## Current Expected Challenge

I expect the hardest part of this project to be separating `analysis` from `hot_take`. NBA comments often include a strong opinion and one piece of evidence, but that does not always mean the post is real analysis. I will need to be consistent about whether the evidence is actually used to explain the claim or just added to make the take sound stronger.
