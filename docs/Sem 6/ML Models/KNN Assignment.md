## Question 1: Evaluation of Cancer Detection ML System

1.1
Given
- Total population = 100,000    
- Actual cancer patients (positive class) = 400
- Healthy individuals (negative class) = 99,600
- Correctly identified cancer patients = 100
- Healthy individuals incorrectly flagged as cancer = 200
- Total wrong predictions = 500

1. TP -> Correct Prediction + Predicted as has Cancer => Patients who have cancer and predicted as has cancer. => 100
2. FP -> Incorrect Prediction + Predicted as has Cancer => Patients who are healthy and predicted as has cancer. => 200
3. TN -> Correct Prediction + Predicted as has no Cancer => Patients who are healthy and predicted as healthy. => 100,000 - 400 - 200 = 99,400
4. FN -> Incorrect Prediction + Predicted as has no Cancer => Patients who has cancer but predicted as has no cancer. => 300

1.2. Clinically more dangerous error

**False Negatives are more dangerous than False Positives.**

**Justification:**

- A false negative means cancer is _not detected_, which can delay diagnosis and treatment.
- Delayed treatment in cancer can significantly reduce survival rates.
- False positives cause stress and additional tests, but usually do not threaten life directly.

---

1.3.
$$Accuracy = \frac{TP+TN}{TP+TN+FP+FN}$$
$$Accuracy = \frac{100+99,400}{100+200+99,400+300}=99.5\%$$

- **Why deceptively high?** Because the dataset is highly imbalanced. Even if the model predicted everyone was "Healthy," it would still achieve 99.6% accuracy

1.4. 
$$Precision = \frac{TP}{TP + FP} = \frac{100}{100 + 200} = 33.3\%$$
- Only 1/3 of people flagged with cancer actually have it. Trustworthiness is low

1.5.
$$Recall = \frac{TP}{TP + FN} = \frac{100}{100+300} = 25\%$$
- The system misses 75% of actual cancer cases, making it very ineffective for screening

1.6.
- Accuracy is high because it is dominated by the massive number of True Negatives (healthy people). Recall is low because the model fails to identify the majority of the small "positive" class

1.7.
- **Recall** is the most useful metric here. In medical screening, the priority is to ensure no actual cases are missed (minimizing False Negatives), even if it means more follow-up tests for False Positives

1.8.
- **Benefit:** The F1-score provides a single harmonic mean of Precision and Recall, penalizing the model for having a very low value in either.
- **Insufficiency:** F1 treats Precision and Recall as equally important. In this clinical case, Recall is far more critical than Precision, so F1 might still mask a dangerously low Recall rate

## Question 2: K-Nearest Neighbors (KNN)

2.1
- Query point: **Q = (3, 3)**
- Distance metric: **Squared Euclidean Distance**

| **Point ID** | **x** | **y** | **Class** | **Distance Calculation** | **$d^2$** |
| ------------ | ----- | ----- | --------- | ------------------------ | --------- |
| **A1**       | 1     | 1     | A         | $(3-1)^2 + (3-1)^2$      | **8**     |
| **A2**       | 1     | 3     | A         | $(3-1)^2 + (3-3)^2$      | **4**     |
| **A3**       | 2     | 2     | A         | $(3-2)^2 + (3-2)^2$      | **2**     |
| **B1**       | 4     | 4     | B         | $(3-4)^2 + (3-4)^2$      | **2**     |
| **B2**       | 5     | 4     | B         | $(3-5)^2 + (3-4)^2$      | **5**     |
| **B3**       | 4     | 5     | B         | $(3-4)^2 + (3-5)^2$      | **5**     |

2.2.
- when k = 1
	- Nearest points are A3 or B1
		- Votes for A = 1
		- Votes for B = 1
	- There is a tie, so we choose the first one => A
	- Prediction = A (tie breaked)
- when k = 3
	- Nearest points are A3, B1, A2
	- Votes for A = 2
	- Votes for B = 1
	- Prediction = A
- when k = 5
	- Nearest points are A3, B1, A2, B2, B3
	- Votes for A = 2
	- Votes for B = 3
	- Prediction = B

2.3.
- A very large k includes points that are far away and likely belong to different clusters. This ignores the local structure of the data and makes the decision boundary too smooth, essentially predicting the majority class of the entire dataset regardless of where the query point is located.

2.4.
**k = 1 is more affected**.

- **Why:** k = 1 makes its decision based solely on the single nearest neighbor. If that one point is mislabeled, the prediction will be 100% wrong. k = 5 is more robust because it averages the votes of 5 points, so one bad point is less likely to flip the final result.

2.5.
- **a) Greatest Contributor:** The neighbors with the **smallest distance** (A3 and B1) contribute the most.
- **b) Reducing Ties:** Yes. Weighted voting makes it much rarer for the sums of weights to be exactly equal, as it accounts for the precise proximity of each point rather than just a simple count.
