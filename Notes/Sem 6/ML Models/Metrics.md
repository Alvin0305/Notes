## Confusion Matrix

|               | Actual Yes     | Actual No      |
| ------------- | -------------- | -------------- |
| Predicted Yes | True Positive  | False Positive |
| Predicted No  | False Negative | True Negative  |
- TP : Caught the thing correctly
- FP : False Alarm
- FN : Missed It
- TN : Correctly Ignored it
## Accuracy

>[!tip] Accuracy refers to "out of everything I predicted, how many did i get right"
>- This includes how much true positives and how much true negatives we caught

$$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$
>[!warning] 
>This fails when the data is unbalanced
>- If positive cases make 99% of the data, then predicting everything as positive gives us a 99% accuracy which is not good

## Precision

>[!tip] Precision refers to "of all the predicted positives, how many are actual positives"
>- It is the number of true positives among the predicted positives

$$Precision = \frac{TP}{TP + FP}$$
## Recall (Sensitivity)

>[!tip] Recall refers to "of all the actual positives, how many did I successfully find"
>- It is the number of true positives among the total number of actual positive cases

$$Recall = \frac{TP}{TP + FN}$$
## Specificity

>[!tip] Specificity is the opposite of Recall
>- It refers to "of all the actual negatives, how many did I correctly ignore"

$$Specificity = \frac{TN}{TN + FP}$$
## F1-Measure

>[!tip] If we care about both precision and recall, use F1.
>- It is low when either of precision or recall is low
>- It is high when both are high
>- It is the Harmonic Mean of precision and recall

$$\text{F1-Measure} = \frac{2 \times Precision \times Recall}{Precision + Recall}$$
