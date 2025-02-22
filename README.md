# Analyzing Breast Cancer Tumour Proteins With K-Nearest Neighbors
## Introduction
I wanted to use my Hackalytics 2025 experience to utilize concepts covered in the Georgia Tech’s CS 6741: Machine Learning course I am currently taking through their OMSCS program. By using the K-Nearest Neighbors (KNN) classifier on a new dataset, I hoped to learn more about best practices and suitability of the classifier for solving different types of classification problems. 
### Dataset
In accordance with Hacklytics Healthcare track, I chose to explore the use of the KNN on the [Breast Cancer Dataset](https://www.kaggle.com/datasets/amandam1/breastcancerdataset). This dataset presents age of diagnosis, stage of tumor, protein levels, and other applicable information for a series patients who underwent surgical tumor removal as part of their cancer treatment plan. 
#### Classification problem and preprocessing
I attempted to emulate a common problem in cancer diagnosis, determining the severity of the cancerous tumor by the levels of expressed proteins. For this purpose, I narrowed my dataset to only consider the four proteins contained in the study (Labelled as Proteins 1-4) and the type of tumor (I, II, or III).
Not a number values were also dropped from this dataset, ensuring predictions would only factor in provided information.
## Suitability assessment
Prior to training the KNN classifier, I chose to assess the suitability of the algorithm through the usage of preliminary statistics and bar graphs. The KNN algorithm relies heavily on the idea that individuals with the same label will resemble each other through their features, so it was important to do research into the distribution of each protein in respect to the tumor stages.\
In order to assess distribution of each protein, I graphed both mean and median protein levels against the different tumor stages. A large difference between mean and median protein levels for the same tumor stage would indicate an imbalance and allow for diagnosis of the skew type. A small or no difference between the two would suggest a balanced dataset, which would be ideal. \
Graphing protein levels also allowed me to assess the general proportions of each protein level according to their tumor stage; Large differences between protein levels per tumor type, or increasing protein levels with the stage of tumor, would be ideal for separating values and allowing for accurate prediction. 
![alt text](https://github.com/krhschuetze/Hacklytics2025BRCA/blob/main/median_proteins.JPG "Median Proteins Per Tumor Stage")
![alt text](https://github.com/krhschuetze/Hacklytics2025BRCA/blob/main/mean_proteins.JPG "Mean Proteins Per Tumor Stage")
In addition to graphs, this data has been visualized as a series of tables below. This makes it easier to assess skew direction for columns shown as imbalanced via bar graph above.

**Protein 1**
| Stage | Mean      | Median    | Difference |
|-------|-----------|-----------|------------|
| I     | -0.01443  | -0.03445  | 0.020      |
| II    | -0.00773  | 0.008438  | -0.016     |
| III   | -0.09422  | 0.052728  | -0.147     |

**Protein 2**
| Stage | Mean      | Median    | Difference | 
|-------|-----------|-----------|------------|
| I     | 1.001318  | 1.039915  | -0.039     |
| II    | 0.964763  | 1.1111    | -0.146     |
| III   | 0.862207  | 0.85337   | 0.009      | 

**Protein 3**
| Stage | Mean      | Median    | Difference |
|-------|-----------|-----------|------------|
| I     | -0.16515  | -0.29285  | 0.128      |
| II    | -0.06541  | -0.12854  | 0.063      |
| III   | -0.08885  | -0.2042   | 0.115      | 

**Protein 4**
| Stage | Mean      | Median    | Difference |
|-------|-----------|-----------|------------|
| I      | 0.037828  | 0.094779  | -0.057     |
| II    | 0.018023  | 0.086514  | -0.068     |
| III   | -0.03145  | -0.02387  | -0.008     | 

#### Analysis
* Protein 1 shows large discrepancies between the two graphs, with a minor skew to the right for stage I tumors and left for stage II. Stage III tumors have a large leftward skew, however. The proportions of protein to tumor types do not correlate as strongly for protein 1 as they do for other protein types.
* The difference between mean and median protein levels for protein 2 indicate this is the most balanced dataset, with only minor skewing. Proportions are also mirrored for both mean and median graphs. However, the protein levels for different tumor types do not have strong differences between them, which might lead to trouble differentiating stages based on this feature.
* Protein 3 skews to the left for all three tumor stages, but proportions are mirrored across graphs and show strong differences. 
* Comparison of mean and median shows all three tumor stages skew slightly to the right for protein 4, however proportions closely match each other for both graphs. The decreasing value associated with each tumor type might also indicate that a falling protein 4 level indicates higher severity disease.
### Takeaways
Protein 1 varies wildly in both value and proportions, indicating imbalance. However, proteins 2-4 appear to resemble each other closely enough that the KNN algorithm should be able to use them effectively. There may be some issues with differentiating tumor stages via protein 2 due to the small differences between tumor types.
## KNN training and optimization
### Scoring
Scoring of the KNN classifier was done via F1 scoring in scikit-learn. F1 scoring was chosen over raw accuracy or loss because it shows the tradeoff between accuracy and recall much better than accuracy alone. This eliminates the possibility of unnaturally high accuracy scores due to overfitting, giving a better idea of true performance. 
### K-nearest neighbors
The k-folds method was used to split training and test data, ensuring that all data was used as both training and test data in turn. Training scores alone came in at a 52.78% average by but testing scores only reached a 33.85% average. The much lower average test score indicates overfitting.
### Optimization
I attempted to combat overfitting by tweaking the number of neighbors considered in the KNN algorithm. A Bayesian search, given the ability to choose any number between one and the number of rows in the entire training fold, ultimately chose to only consider 1 neighbor. The f1 score for this value was equal to the original 33.85% average, so I decided to do a little more investigation into the subject via validation graph, mapping values 1-301 in intervals of 10 to save computing power.
![alt text]( https://github.com/krhschuetze/Hacklytics2025BRCA/blob/main/neighbors_validation.JPG "N Neighbors Validation Graph")
The validation graph showed that the benefit gained by increasing the number of neighbors rapidly declined from one onward, providing virtually no benefit after 40 neighbors. 
### Analysis
Despite the value of neighbors considered serving as the basis for the KNN algorithm, tweaking this hyperparameter did little to improve its performance. This implies the low score and overfitting are due to other factors.
## Limitations
The biggest limitation on the performance of the KNN was the time restriction- I completed this project and write up over the span of ~6 hours. With more time, I would have liked to complete a more through hyperparameter tuning that explored which hyperparameters would affect the performance of the KNN classifier, and why its namesake did not. \
An additional direction I would like to explore to attempt to limit the KNN’s overfitting on this data would be limiting the number of training epochs- This is one of the most common ways to deal with overfitting, and for good reason.\
Another limitation came in the form of the dataset itself- The dataset only contained data from individuals with confirmed cancerous tumors, and left the measured proteins abstracted for privacy. This left me unable to compare protein levels to individuals without cancerous tumors, which eliminates the possibility of using this technology to diagnose cancerous tumors vs noncancerous based on measured levels of relevant proteins.  
## Conclusion
While I was unable to reach a high F1 score within the time allotted to this project, I still learned valuable skills I can apply to other data science projects in the future. 

