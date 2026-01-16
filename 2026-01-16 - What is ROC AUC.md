---
tags:
  - machine-learning
---
# What is ROC AUC?

**Performance metric for a binary classification model.**

**Intuition: Higher ROC AUC = better separation between positive and negative classes. i.e. Positive samples tend to get higher scores than negative ones.**

Etymology
- ROC = Receiver Operating Characteristic (relic from WWII radar engineering).
- AUC = Area Under Curve.
- ROC AUC = The area under the ROC curve.

Context
- Binary classification models usually output a continuous value (e.g. 0.83) rather than either 1 or 0.
- We need a decision threshold (e.g. > 0.8) to convert its output to a binary label.

**The ROC curve plots a model's True Positive Rate (TPR) against its False Positive Rate (FPR) across all decision thresholds.**
- TPR = TP / TP + FN (_aka recall_)
- FPR = FP / FP + TN (_note: denominator is total actual negatives_)

Example ROC (better curves have larger AUC):

![[roc_illustration.png]]

Notes:
- Both TPR and FPR should decrease as the decision threshold increases (from top right to bottom left corner).
    - Intuition - Higher threshold = stricter = less positive predictions = lower recall but also lower FPR.
- Inverse L shaped curves are better because TPR decreases much slower than FPR. In other words, we maintain TPR while lowering FPR as the decision threshold increases.
- A perfect classifier would have a point at (0, 1).