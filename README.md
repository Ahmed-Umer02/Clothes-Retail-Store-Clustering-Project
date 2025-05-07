# Clothes Retail Store Clustering Project

## Introduction

In this project, we explore the application of clustering algorithms to segment retail transaction data from multiple zones across five years (2018–2022). The goal is to identify meaningful patterns in sales behavior, inventory management, and customer engagement, which can assist in strategic decisions such as marketing, stock allocation, and customer targeting.

#### Problem Overview

Retail businesses often collect vast amounts of transaction data that can provide valuable insights into customer purchasing behavior, product performance, and store operations. However, making sense of this data can be challenging without proper analysis techniques. This project uses clustering methods to group similar transactions and uncover hidden patterns that can inform business decisions.

#### Clustering Algorithms

To segment the data, we implemented and compared four popular clustering algorithms:

- **K-Means Clustering**
- **Fuzzy C-Means Clustering**
- **Single-Linkage Hierarchical Clustering**
- **Self-Organizing Maps (SOMs)**

These algorithms offer varying approaches to clustering, from deterministic (like K-Means) to probabilistic (like Fuzzy C-Means) and hierarchical (like Single-Linkage), each with its own strengths and weaknesses.

#### Data Overview

The data set contains transaction records with features such as `line_item_amount`, `bill_discount`, `zone_name`, `store_name`, and product categories. After preprocessing, including handling missing values and feature extraction, the data was divided into two subsets: one with 500,000 rows and another with 1 million rows. For computational efficiency, we used the 500,000-row dataset in the final analysis.

#### Objective

The objective of this project is to apply and evaluate various clustering methods to determine the most effective algorithm for segmenting retail transaction data. The results of these clustering analyses can provide insights into customer behavior, sales patterns, and opportunities for optimization in retail operations.



## Data

The dataset used in this project consists of retail transaction records from the years **2018 to 2022**, originally sourced from [Kaggle](https://www.kaggle.com/datasets/nishchay331/retail-store). Each yearly CSV file was cleaned and combined into a single dataset for analysis.

#### Data Cleaning and Preprocessing

Key preprocessing steps included:

- **Combining yearly files**: CSVs for each year were loaded using `pandas` and concatenated into a single DataFrame.
- **Handling missing values**: Due to the large dataset size, one-hot encoding was not feasible. Instead, missing values were imputed using the placeholder `'Unknown'`. An alternative strategy using the most frequent value was implemented but not used.
- **Feature engineering**: Extracted `transaction_day` and `transaction_month` from the `transaction_date`, and converted relevant fields like `line_item_amount` to `float`.
- **Label encoding**: Categorical columns (e.g., `description`, `inventory_category`, `colour`, etc.) were encoded using `LabelEncoder` to prepare for model training.

#### Dataset Versions

Two versions of the cleaned dataset were created:

- `data_1m.zip`: A sample of 1 million transactions, evenly split across five years. This version was intended for large-scale experimentation but was **not used in modeling** due to computational constraints.
- `data_500k.zip`: A sample of 500,000 transactions (100,000 per year), which was used for **all model training, evaluation, and visualizations**. This subset provided a balance between scale and feasibility.

Both versions were saved as compressed CSV files, and Apache Arrow was used for efficient file writing.

#### Exploratory Analysis

The `500k` dataset was used for exploratory analysis, including:

- **Boxplots and violin plots**: To examine the distribution of `line_item_amount` and `bill_discount` across zones.
- **Heatmaps**: To visualize correlation between numerical features.
- **Outlier detection**: Boxplots were used to reveal anomalies in sales across different zones.
- **Sales trends over time**: Monthly sales were grouped and plotted by `zone_name` to identify patterns from 2018 to 2022.

These steps ensured a rich understanding of the data before applying clustering algorithms.


## Model Training and Evaluation

This project implements and compares four clustering algorithms on a dataset of retail transactions:

- **K-Means Clustering**
- **Fuzzy C-Means Clustering**
- **Single-Linkage Hierarchical Clustering**
- **Self-Organizing Maps (SOMs)**

#### Preprocessing

The dataset undergoes the following preprocessing steps:
- Categorical features are encoded using `LabelEncoder`
- Numerical features are standardized using `StandardScaler`
- Data is split into **70% training** and **30% testing**

#### Models

##### 1. K-Means
- Trained with `n_clusters=5`
- Initialized using k-means++ with a fixed random seed

##### 2. Fuzzy C-Means
- Implemented using the `skfuzzy` library
- Uses fuzzy partitioning and computes cluster probabilities for each sample

##### 3. Single-Linkage
- A form of Agglomerative Hierarchical Clustering
- Utilizes single linkage (minimum distance) criterion

##### 4. Self-Organizing Maps (SOM)
- A 2D lattice of neurons (5x5 grid)
- Trained using unsupervised competitive learning (`MiniSom`)

#### Evaluation Metrics

Each model is evaluated on both **training** and **test sets** using:

- **Silhouette Score**: Measures cohesion and separation of clusters  
- **Davies-Bouldin Index**: Assesses intra-cluster similarity and inter-cluster differences

#### Results Summary

| Model                  | Silhouette (Train) | DBI (Train) | Silhouette (Test) | DBI (Test) |
|------------------------|--------------------|-------------|-------------------|------------|
| K-Means                | *0.1271*           | *1.9484*    | *0.1266*          | *1.9518*   |
| Fuzzy C-Means          | *-0.0088*          | *2.4275*    | *-0.0348*         | *2.7841*   |
| Single-Linkage         | *0.9096*           | *0.0749*    | *0.9199*          | *0.05452*  |
| Self-Organizing Maps   | *0.0642*           | *2.1026*    | *0.0647*          | *2.1081*   |


#### Visualizations

- Heatmaps of cluster assignments
  ![image](https://github.com/user-attachments/assets/d837468c-e278-4e1b-98e4-154e371e3633)

- Silhouette score distributions per model
![image](https://github.com/user-attachments/assets/e759f94c-b7b2-43e3-bcb6-0a2cfb9eaefe)

- Cluster size distributions
![image](https://github.com/user-attachments/assets/74059a65-30c7-4282-8ee6-65a994862011)

These visuals help interpret how each model groups the data and how well-separated the clusters are.

## Conclusion

This project explored four clustering algorithms—**K-Means**, **Fuzzy C-Means**, **Single-Linkage Hierarchical Clustering**, and **Self-Organizing Maps (SOMs)**—to segment retail transaction data. The results revealed important insights into their strengths and limitations:

- **Single-Linkage Hierarchical Clustering** achieved the **best performance** across all evaluation metrics. It yielded the **highest Silhouette Scores** and **lowest Davies-Bouldin Index** on both training and test sets, indicating very cohesive and well-separated clusters. However, it is known to be sensitive to noise and may suffer from the "chaining effect" in other datasets, which can produce elongated clusters.

- **K-Means Clustering** provided a **fast and efficient baseline**, producing moderately separated clusters with consistent results between train and test sets. While it assumes spherical clusters and equal variance, its simplicity and scalability make it a strong choice for many practical applications.

- **Fuzzy C-Means** produced overlapping clusters and underperformed on both evaluation metrics. Although it allows for **soft clustering** (each point belongs to multiple clusters with probabilities), the results suggest that the data may not benefit significantly from this flexibility, or the parameters need further tuning.

- **Self-Organizing Maps (SOMs)** offered a **grid-based, topology-preserving clustering**, useful for visualizing and understanding the underlying data structure. However, its performance was lower than K-Means and Single-Linkage, likely due to the complexity of training and sensitivity to initialization and grid size.

### Practical Implications

- **Retail Operations**: The clustering results can help businesses optimize inventory distribution, tailor marketing strategies, and better understand customer preferences by grouping transactions into distinct segments.
  
- **Future Work**: While the models used in this project show promising results, there are several areas for improvement:
  - **Hyperparameter Tuning**: Further fine-tuning the number of clusters and other hyperparameters could improve model performance, particularly for K-Means and Fuzzy C-Means.
  - **Scalability**: Hierarchical and SOM methods could benefit from optimizations to handle larger datasets more efficiently, possibly by parallelizing training processes or using dimensionality reduction techniques.
  - **Feature Engineering**: Incorporating additional features, such as customer demographics or product-specific data, could further enhance clustering performance and lead to even more targeted insights.

### Final Thoughts

For this dataset, **Single-Linkage Clustering** stood out as the most effective method, producing the most distinct and cohesive clusters. However, for scalability and ease of deployment in production environments, **K-Means** remains a reliable option. While **Fuzzy C-Means** and **SOMs** bring unique interpretability and flexibility, they may require further tuning or be more appropriate for use cases with overlapping or complex data structures.


