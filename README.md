# Separating_the_Sheep_from_the_Goats
A project aimed at distinguishing genuine banknotes from counterfeit banknotes using logistic regression in Python

***

**PROJECT OBJECTIVE AND SELECTED LIBRARIES**

:dart: The objective of [this project](https://github.com/Praemuntiacus/Separating_the_Sheep_from_the_Goats/blob/main/CROITOR_Roman_1_code_0622.ipynb) is to analyze a dataset containing parameters of "true banknotes" and "false banknotes" in order to develop a model that can accurately distinguish between the two. Some parameters in the dataset were partially missing. For the purpose of this project, I utilized the **Statsmodels** library to describe and estimate the results of linear regression. Data preprocessing and K-means clustering were performed using the **Scikit-Learn** library. Logistic model development was also carried out using **Scikit-Learn**.

:bar_chart: For visualization purposes, I employed the **Matplotlib** and **Seaborn** libraries, as well as the **parallel_coordinates** function from **pandas.plotting**. To gain an initial understanding of the data, I utilized **seaborn.PairGrid()** with the '*hue*' parameter set to "*is genuine*". This provided insights into the differences between true and false banknotes and helped identify the most suitable parameters for distinguishing banknotes. During data exploration, it became evident that a small portion of the data (approximately 2.5%) was incomplete and missing one parameter.
***

**DATA IMPUTATION**

To address the issue of missing data, I created separate subsets for complete and incomplete data. Since both subsets contained information on true and false banknotes, I decided to perform data imputation separately for each banknote category. To achieve this, I created new sub-subsets for true and false banknotes. For visualization purposes, simple linear regression was employed for data imputation, although multiple linear regression could have been a viable alternative. The *Ordinary Least Square* method was used to estimate the parameters of the relationship between the independent variable (predictor) and the dependent variable (response). The results of the application indicated that the covariance of the datasets was not robust, and the coefficient of determination **R2** was very low, suggesting a limited predictive capacity of the independent variable. Nevertheless, for the purpose of data imputation, the parameters of linear regression proved satisfactory. The **Root Mean Squared Error** (RMSE) obtained was relatively low, indicating that the model's predictions aligned well with the actual values.
***

**ASSESSMENT OF QUALITY OF DATA IMPUTATION**

The histograms of the residuals from the linear models exhibited a normal distribution, implying unbiased and accurate models and supporting the validity of the model assumptions. The assumption of homoscedasticity was supported by the p-value of the *Breusch-Pagan test*, which indicated the absence of sufficient evidence to suggest heteroscedasticity in the regression model residuals. With a p-value greater than 0.05, we would fail to reject the null hypothesis of no heteroscedasticity. This finding suggests that there is insufficient evidence to conclude that the residuals exhibit significant heteroscedasticity. Homoscedasticity is an important assumption in linear regression analysis as it ensures valid statistical inference, reliable prediction intervals, and consistent interpretation of coefficients.

Hence, the resulting regression models were suitable for predicting missing values of both true and false banknotes. The predicted values were assigned to the column with missing data in the incomplete dataframes, which were then appended to the complete data subsets.
***

**K-MEANS CLUSTERING**

To distinguish between true and false banknotes, I employed the K-means algorithm. Data preprocessing involved excluding columns with data that were not suitable for clustering (such as strings) and normalizing numerical data using the **StandardScaler** from the **scikit-learn.preprocessing** module.
The optimal number of clusters (typically two clusters are expected) was determined using the *silhouette score* and the "*elbow method*".
The K-means clustering process was executed by fitting the dataset to the KMeans tool with pre-established parameters, including the number of clusters. This generated a '*cluster*' object containing an array of cluster numbers.

To visualize the resulting clusters, I applied dimensionality reduction using **principal component analysis** (PCA) to reduce the dimensions to two principal components. The transformed array of two principal components was converted into a dataframe, and an additional column was created to store the data from the 'cluster' object. The dimensionality reduction using 'PCA' was also applied to the cluster centers (**kmeans.cluster_centers_**). The results of the PCA data transformation enabled the construction of a two-dimensional scatter plot illustrating the well-separated clusters and their centers.
To assess the accuracy of the K-means clustering, I estimated the error rate. To do this, I added the column from the initial dataframe used in the K-means clustering, containing information on whether the banknote is true or false, to the resulting dataframe with the cluster column. The obtained **positive predictive value** (0.99), **negative predictive value** (0.972), **sensitivity** (0.986), and **specificity** (0.98) demonstrate that the algorithm effectively separates the data points and captures the underlying patterns or similarities in the data. It is worth noting that the model makes slightly more errors in the case of false banknotes, possibly due to the parameters of false banknotes being more dispersed and thus more challenging for the clustering algorithm to discern accurately.
***

**FUNCTION DISTINGUISHING 'TRUE' AND 'FALSE' BANKNOTES**

Using the results of clustering, I developed a *function* called '**predict_banknote**' based on the centroids defined by K-means clustering. This function calculates the distances between each banknote and the centroids, determines the closest centroid for each banknote, and prints the classification result based on the closest centroid. Here is an overview of the steps involved:
•	In the first line of the code, I converted the inputs (parameters of banknotes to verify their authentity) into **NumPy** arrays to facilitate manipulation using **NumPy**.
•	Next, I created an empty list called '*distances*' to store the calculated distances between each banknote instance and the centroids.
•	Then, I implemented a nested loop using '**for ... in**' to calculate the squared Euclidean distance between each banknote instance and each centroid. The calculated distances are appended to the 'distances' list.
•	Afterward, I reshaped the '*distances*' list into a matrix-like object that corresponds to the structure of the data to be tested.
•	Subsequently, I created a list called '*closest_centroid*' that contains the cluster number of the corresponding centroid found for each banknote instance using '**np.argmin(dist)**'.
•	Finally, I implemented another loop using '**for ... in**' to sort the banknote instances as either true or false (**if ... else**) and print the results.

:date: The second variant of this function, called '*predict_array*,' returns the results as a dataframe.

<p align="center">
  <img src="https://github.com/Praemuntiacus/Separating_the_Sheep_from_the_Goats/blob/main/10_before_pca.jpg" alt="before pca reduction">
</p>

<p align="center">
  <i>Principal components before dimensionality reduction</i>
</p>

***

**PRINCIPAL COMPONENT ANALYSIS**

In order to understand the distinctions between the samples of true and false banknotes, I performed principal component analysis (**PCA**) here. After scaling the data, I added a new column to the dataset and assigned the values from the '*clusters*' object, which contains the cluster number for each banknote instance. The **parallel_coordinates()** function was particularly useful for visualizing the data after dimensionality reduction using **PCA**. According to the **parallel_coordinates()** diagram, reducing the dimensionality from six to five principal components is optimal.
Determining the number of retained principal components that capture a sufficient amount of explained variance was done using the **Scree Plot** and **Eigenvalues**. These demonstrate that while the first principal component captures the majority of explained variance, it is insufficient to explain a reasonably high amount. Consequently, I retained the first four principal components for analysis and interpretation. The first factorial plan with PC1 and PC2 exhibits the best distinction between the two clusters of banknotes. The circle of correlations reveals that the first factorial plan reflects the parameters quite well (diagonal), while other parameters describing the height and length of banknotes are not fully represented in the first factorial plane. On the other hand, the second factorial plane, which reasonably represents the banknote heights according to the **circle of correlations**, is somewhat confusing and does not provide significant insights.

<p align="center">
  <img src="https://github.com/Praemuntiacus/Separating_the_Sheep_from_the_Goats/blob/main/10_after_pca.jpg" alt="after pca reduction">
</p>

<p align="center">
  <i>Principal components after dimensionality reduction</i>
</p>


Furthermore, I present combined plots of factorial plans and the circle of correlations. To achieve this, I first normalized the PCA-transformed data, as the diameter of the hypersphere in the circle of correlations equals 1. This normalization ensures that all variables are on a comparable scale. Next, I overlaid the scatter diagram of the normalized data from the factorial plans and represented the variable projections using arrows.
***

**LOGISTIC REGRESSION**

The technique of logistic regression is applied in order to distinguish "false" and "true" banknotes.
***
