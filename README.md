
# K means clustering ML Group 2

This repository contains the codes and docs for our implementation of K means clustering.


## Table of Contents

1. [Dataset Overview](#dataset-overview)
2. [🛠️ Model Training](#-model-training)
   - [Data Cleaning and Transformation](#data-cleaning-and-transformation)
   - [Feature Scaling](#feature-scaling)
   - [Custom K-Means Implementation](#custom-k-means-implementation)
   - [Determining Optimal k Using the Elbow Method](#determining-optimal-k-using-the-elbow-method)
3. [🧪 Model Testing](#-model-testing)
   - [Manual Labeling for Ground Truth](#manual-labeling-for-ground-truth)
   - [Label Matching via Hungarian Algorithm](#label-matching-via-hungarian-algorithm)
   - [Evaluation Metrics](#evaluation-metrics)
   - [Confusion Matrix Visualization](#confusion-matrix-visualization)
4. [Results](#results)
   - [📊 Part 1](#-part-1)
     - [🌟 Key Learnings From K=3](#key-learnings-from-k3)
     - [🎨 PCA-Powered Visualization](#pca-powered-visualization)
     - [📊 Performance Showdown](#performance-showdown)
   - [📊 Part 2](#-part-2)
     - [🌟 Key Learnings From K=2](#key-learnings-from-k2)
     - [🎨 PCA-Powered Visualization](#pca-powered-visualization-2)
     - [📊 Performance Showdown](#performance-showdown-2)
5. [🛠️ Behind the Scenes](#-behind-the-scenes)
6. [🚀 Future Explorations](#-future-explorations)
7. [Authors](#authors)


## Dataset overview

This dataset contains mobile app usage data, capturing key numerical attributes that provide insights into user behavior. Each record consists of the following attributes:
App: Name of the mobile application.
Usage (minutes): Total time spent on the app in minutes.
Notifications: Number of notifications received from the app.
Times Opened: Number of times the app was accessed.

This dataset is useful for mobile app engagement analysis due to the following characteristics:
Continuous Numerical Attributes: The dataset consists of numerical values, making it suitable for various analytical and machine learning models.
User Behavior Insights: The data can help identify patterns in app engagement, such as which apps users spend the most time on or receive the most notifications from.
Feature Engineering Potential: Additional features, such as average session duration or notification-to-open ratio, can be derived to enhance analysis.
Trend Analysis: The dataset can be used to track user habits and detect trends in mobile app usage over time.

In conclusion, this dataset serves as a valuable resource for analyzing mobile app usage patterns, providing insights into user interaction and engagement that can be leveraged for research or business decision-making.

Original dataset: https://www.kaggle.com/datasets/anandshaw2001/mobile-apps-screentime-analysis

Modified dataset: https://drive.google.com/file/d/1Wu4mcdsiurCL4gDuOCP1Ij8Vq11qI3wR/view?usp=sharing

 ## 🛠️ **Model Training**  

Before training, the dataset undergoes data preprocessing to prepare it for clustering:

- **Data Cleaning and Transformation**:
The dataset contains a mix of numerical features and non-numeric identifiers. Non-numeric fields such as App names or Date values are removed to ensure the clustering algorithm only operates on numerical data. This avoids type errors and focuses the model on quantitative engagement behaviors.

- **Feature Scaling**:
The remaining numerical features (such as Usage (minutes), Notifications, and Times Opened) are standardized using StandardScaler from scikit-learn. This transformation converts all features to a common scale (mean = 0, standard deviation = 1), which is important because K-Means uses Euclidean distance and is sensitive to differences in feature magnitude.

- **Custom K-Means Implementation**:
Rather than using a built-in implementation from libraries like sklearn.cluster.KMeans, we built our K-Means algorithm from scratch in Python.

This includes:

- **Random initialization of centroids**

- **Distance calculation using Euclidean norm**

- **Assignment of each data point to the nearest cluster**

- **Updating centroids as the mean of all assigned points**

- **Iterative repetition until centroids stabilize (convergence)**

Determining Optimal k Using the Elbow Method:
To find the best number of clusters (k), we apply the Elbow Method.

This involves:

- **Running K-Means with different values of k (from 1 to 9)**

- **Calculating the Within-Cluster Sum of Squares (WCSS) for each k**

- **Plotting the WCSS values to visually identify the 'elbow'—the point where adding more clusters no longer provides a significant improvement**

In our case, k = 2 was selected, aligning with our goal to separate apps into High Engagement and Low Engagement categories.



 ## 🧪 **Model Testing**

Although K-Means is an unsupervised algorithm (meaning it doesn't use labels during training), we introduce a **manual validation step** to test how well the clustering aligns with real-world categories:

- **Manual Labeling for Ground Truth**:  
  Certain applications are pre-identified based on common knowledge:

  - **High Engagement (Label = 0)**: e.g., Instagram, WhatsApp  
  - **Low Engagement (Label = 1)**: e.g., Safari, 8 Ball Pool  

  These labels are not used during training, but are introduced after clustering for evaluation purposes.

- **Label Matching via Hungarian Algorithm**:  
  Since K-Means assigns arbitrary cluster numbers (e.g., Cluster 0 may represent Low or High Engagement), we use the **Hungarian Algorithm** (from `scipy.optimize.linear_sum_assignment`) to optimally match the predicted clusters to the true labels based on maximum overlap. This ensures a fair and meaningful comparison.

- **Evaluation Metrics**:  
  After aligning clusters with ground truth labels, we assess clustering performance using standard classification metrics:

  - **Accuracy**: Proportion of correct classifications  
  - **Precision**: Correctness of positive predictions  
  - **Recall**: Coverage of actual positive cases  
  - **F1 Score**: Harmonic mean of precision and recall  

- **Confusion Matrix Visualization**:  
  A confusion matrix is generated and visualized using **Seaborn’s heatmap**, providing insight into:

  - **Correct classifications** (diagonal cells)  
  - **Misclassifications** (off-diagonal cells)  

  This visual aid supports interpretation of model behavior and error patterns.

## Results
# 📊 Part 1
## 🌟 **Key Learnings From K=3**  
- **Initial Misstep**: **K=3** was chosen based on a misread of the elbow graph. 
- **Class Confusion**: Particularly with class 1 (low recall and many misclassifications).
- **Data Preprocessing Helped**: Modified dataset showed clearer clustering and performance gains.
- **Lesson Learned**: The elbow method must be paired with interpretability checks.
### 🎨 **PCA-Powered Visualization**  
<img src="./plots/ClusteringVisual2.png" alt="PCA Plot" width="600"/>

- **🔵 Blue Cluster**: Label 0 based on hungarian algorithm.
- **🟠 Orange Cluster:** Label 1 based on hungarian algorithm.
- **🟢 Green Cluster:**  Label 2 based on hungarian algorithm.
- **⭐ Centroids**: Nestled in dense regions, confirming their authority.  

## 📊 **Performance Showdown**  
### 🏆 **Original Dataset (K=2)**  
| Metric       | Score   | Verdict                          |  
|--------------|---------|----------------------------------|  
| Accuracy     | 60.50%  | Low accuracy, poor clustering.   |  
| Precision    | 64.85%  | High false positives.            |  
| Recall       | 60.50%  | Missed many class 1 points.      |  
| F1-Score     | 57.28%  | Unbalanced performance.          |  

### 🚀 **Modified Dataset (K=2)**  
| Metric       | Score   | Verdict                          |  
|--------------|---------|----------------------------------|  
| Accuracy     | 69.00%  | Better, but still inconsistent.  |  
| Precision    | 72.45%  | Fewer mislabels.                 |  
| Recall       | 69.00%  | Improved detection.              |  
| F1-Score     | 67.89%  | Balanced, but not optimal.       |  

---

# 📊 Part 2    


## 🌟 **Key Learnings From K=2**  
- **Optimal Clusters (K=2)**: Discovered natural groupings of apps into **High** and **Low Engagement** with **97% accuracy** (modified dataset).  
- **PCA Magic**: Visualized clusters in 2D space—Principal Component 1 revealed the dominant split between engagement levels.  
- **Metrics That Shine**:  
  - **Precision**: Up to **97.17%** (almost no false alarms!).  
  - **Recall**: **97%** (nearly all High Engagement apps correctly tagged).  
  - **F1-Score**: **97%** (perfect harmony of precision and recall).  


### 🎨 **PCA-Powered Visualization**  
![Final Visualization Plot of App Engagement Clusters](./plots/ClusteringVisual.png)  
- **🔴 Red Cluster**: Low Engagement apps (left side)  
- **🔵 Blue Cluster**: High Engagement apps (right side)  
- **⭐ Centroids**: Nestled in dense regions, confirming their authority  
- **Hybrid Apps?** A few rogue red dots near the blue cluster - possibly transitional engagement cases


## 📊 **Performance Showdown**  
### 🏆 **Original Dataset (K=2)**  
| Metric       | Score   | Verdict                          |  
|--------------|---------|----------------------------------|  
| Accuracy     | 93.50%  | Rock-solid!                      |  
| Precision    | 93.68%  | Few false positives.             |  
| Recall       | 93.50%  | Rarely misses true High Engagers.|  
| F1-Score     | 93.56%  | Balanced and reliable.           |  

### 🚀 **Modified Dataset (K=2)**  
| Metric       | Score   | Verdict                          |  
|--------------|---------|----------------------------------|  
| Accuracy     | 97.00%  | Near-perfect!                    |  
| Precision    | 97.17%  | Almost no mislabels.             |  
| Recall       | 97.00%  | Flawless capture of High Engagers.|  
| F1-Score     | 97.00%  | Precision + Recall = ❤️          |  

**Why the boost?** Cleaner data, fewer outliers, and sharper cluster boundaries!  

---

## 🛠️ **Behind the Scenes**  
- **Tools Used**: Python, NumPy, Pandas, Matplotlib, PCA.  
- **Algorithm**: Manual K-Means (no `sklearn` shortcuts!).  
- **True Labels**: Crafted from app usage quantiles (High/Low Engagement).  

---

## 🚀 **Future Explorations**  
- **Automate K-selection** with the `kneed` library.  
- **Battle of Algorithms**: Test DBSCAN vs. K-Means on messy data.  
- **Real-World Labels**: Partner with app developers for ground-truth validation.  

---

*✨ Data tells stories. We just listened.*  

## Authors

- TRISHA MAY GONZALES
- EDRICK CANZANA
- JOBERT JOHN CAYAO
- VICTOR MANUEL DALISAY
- JONER PAUL DE SILVA
- JOHN MICHAEL GABALFIN
- DOMINIC REYMAR GUNIO
- MARIUS JACOB HERNANDEZ



