# [DL4SE] Data Mining Analyses 

> *Last update:* March 2021; *Maintaned by* @danaderp

Before carrying out any mining tasks, we decided to address a basic descriptive statistics procedure for each feature. These statistics exhibit basic relative frequencies, counts, and quality metrics. We revised 4 quality metrics: ID-ness, Stability, Missing, and Text-ness. ID-ness measures to what extent a feature resembles an ID -the “title” is the only feature with a high ID-ness value. Stability measures to what extent a feature is constant (or have the same value). Missing is the number of missing values. Finally, Text-ness measures to what extent a feature contains free-text. The results for each feature can be navigated once the SRL process is run in RapidMiner. As for the data mining tasks employed, we focused on three of them: Correlations, Association Rule Learning, and Clustering.

## Correlations by Mutual Information
Due to the nominal nature of the SLR features, we were unable to perform classic statistical correlations on our data (i.e. pearson’s correlation). However, we adapted an operator based on attributes’ information dependencies. This operator is known as “mutual information” [(MacKay, 2005)](https://www.inference.org.uk/itprnn/book.pdf). Similar to covariance or pearson’s correlation, we were able to represent the outcomes of the operator in a confusion matrix (for further details please run the RapidMiner process).

![Mutual Information Matrix of DL4SE](https://wm-semeru.github.io/dl4se/results/dataset2021/correlation/Mutual_Information_Matrix.png)

The mutual information measures to what extent one feature knows about another one. High mutual information values represent less uncertainty; therefore, we built arguments like “the deep learning architecture used on a paper is less uncertain (or more predictable) given a particular SE task” or  “the reported architecture on the papers are mutually dependent with the Software Engineering task”. 

The following relationships are strong correlated by mutual information. It shows us how much one attribute tells us about another one. A high mutual information means a large reduction of uncertainty. 
- Venue ⟷ SE Problem [1.70] | Combat Overfitting [1.34]  | Data Preprocessing [1.21] | SE Data [1.08]
- SE Problem ⟷ SE Data [1.61] | Data Preprocessing [1.56] | Combat Overfitting [1.51] | [data] Code [1.29] | Loss Function [1.24] | Architecture [1.16] | Metrics [1.03]
- Data Preprocessing ⟷ Combat Overfitting [1.33] | Architecture [1.04] | [data] Code [1.09] | SE Data [1.37]
- SE Data ⟷  [data] Code [1.13] | Combat Overfitting [1.05]
- Loss Function ⟷  Combat Overfitting [1.04]


### Self-Information (or Entropy-based) features

| Most Informative Features | Self-Inf Value| Less Informative Features | Self-Inf. Value|
| --- | --- | --- | --- | 
| SE Problem | 3.95 | Roc&AUC | 0.38 |
| Combat Overfitting | 3.66 | [over] Data Balancing | 0.34 |
| Venue | 3.65 | [pre] I/O Vectors | 0.34 |
| Data Preprocessing | 3.27 | [metric] Approx Error | 0.27 |
| Loss Function | 3.05 | [over] Data Augmentation | 0.27 |


The difference between correlations and associations rules depends on the granularity level of the data (e.g., paper, feature, or class). The correlation procedure was performed at the feature level, while the association rule learning was performed at the class or category level.

## Association Rule Learning
For generating the association rules, we employed the classic FP-Growth algorithm (FP stands for Frequent Pattern). The FP-Growth computed frequently co-occurring items in our transactional dataset (see DL4SE-Dataset.csv) with a min support of 0.95. These items comprise each class per feature. For instance, the feature “Loss Function” exhibits a set of classes (or items) such as “NCE”, “Max Log Likelihood”, “Cross-Entropy”, “MSE”, “Hinge Loss”, “N/A-Loss”, and so on. The main premise of the FP-Growth can be summarized as: if an itemset is frequent (i.e. {MSE, RNN}),  then all of its item subsets are also frequent (i.e. {MSE} and {RNN}).

Once the FP-Growth generated the itemsets (e.g. {MSE, Hinge Loss}), the algorithm started to check the itemsets’ support in continuous scans of the database. Such support measures the occurances of the itemset in the database. The FP-Growth scans on the DB brought about a FP-tree data structure. We recursively mined the FP-tree to extract frequent itemsets [(Han, et al; 2000)](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.40.4436). Nonetheless, the association rule learning requires more than the FP-tree extraction.

![Association Rule Learning of DL4SE](https://wm-semeru.github.io/dl4se/results/dataset2021/association/AssociationRules.png)

An association rule serves as an if-then statement (or premise-conclusion) based on the frequent itemset pattern. Let’s observe the following rule mined from our dataset: “Given that an author used Supervised Learning, we can conclude that their approach is irreproducible with a support of 0.7 and a confidence of 0.8”. We observe the association rule has an antecedent (i.e., the itemset {Supervised Learning}) and a consequent (i.e.,the itemset {Irreproducible}). These relationships are mined from itemsets that usually have a high support and confidence. The confidence is a measure of the number of times that premise-conclusion statement is found to be true. We tuned both parameters to be greater than 0.7.

We refute the idea that association rule learning avoids spurious correlations. Nonetheless, we organized the rules into an interconnected net of premises/conclusions to find explanations around techniques and methodologies reported on the papers. Any non-logical rule is disregarded as well as rules that possess lower support and confidence. 

### Association Rules Reasoning [2020 results]
1. **A reproducibility argument**: For most of the papers that exclude vision and repo meta, input/output examples, or natural language based data, the authors fail to provide enough data to reproduce their approach with confidence levels above ~0.82.
2. **A learning type argument**: For most of the papers that use Supervised learning, the authors omit to report ROC or AUC evaluations, data extraction details, and are irreproducible with confidence levels above ~0.87.
3. **A preprocessing argument**: In the papers that exclude vision or repo meta based data, most of the authors fail to report how they preprocessed their data with confidence levels above ~0.81.
4. **The data extraction argument**: Many of the papers that exclude input/output examples, vision, natural language, or repo meta based data omit how their data was extracted with confidence levels above ~0.82.
5. **A learning algorithm argument**: For most of the papers that exclude natural language or repo meta based data, the authors fail to discuss the learning algorithm employed with confidence levels around ~0.82. 

## Clustering
Clustering operators group or segment instances of data that are similar to each other and -at the same time- differentiable from other instances in other clusters. Additionally, this mining task describes datasets that are unlabeled by identifying a prototype or an instance that represents the entire cluster. For our EDA, we carried out a segmentation of papers using the k-medoids algorithm [(Kaufman & Rousseeuw; 1987)](https://wis.kuleuven.be/stat/robust/papers/publications-1987/kaufmanrousseeuw-clusteringbymedoids-l1norm-1987.pdf). We wanted to make sure that the centroids of the generated clusters are actual points (papers) from the dataset. 

We built two distinct pipelines in RapidMiner for the paper grouping: 1) a PCA clustering and 2) an explainable clustering. On the one hand, the goal of the first pipeline is to visualize the points (or papers) in a reduced dimensionality (two pcs components). The PCA clustering allowed us to find the maximum change in variation when testing different k clusters values. The PCA clustering shows that 4 clusters were ideal for the analysis. On the other hand,  the goal of the second pipeline is to identify which features are influencing the segmentation. Knowing these features allowed us to explain how the algorithm was segmenting the space. Finally, we provided the descriptions of four types of papers found during the clustering analysis.  

### K-means and PCA Reduction
![Clustering Description](https://wm-semeru.github.io/dl4se/results/dataset2021/clustering/Clustering%20PCA.PNG)

### Explainable Clustering
Using PCA for clustering fails to provide explainable features for clustering decision. Therefore, we used all features witout any trasformation and then perform K-Meoids algorithm with nominal measures and Kulczynski Similarity. The following plot depicts the cetroids values to determine which features contributes the most for segmenting the papers.
![Centroids Plot](https://wm-semeru.github.io/dl4se/results/dataset2021/clustering/Clustering%20Centroids.PNG)
#### Influential Features
The following features are contributing the most for segmenting papers. You can observe in the previous plot the distance exhibit for each feature respect to the centroid value. 
- SE Problem
- Architecture
- Data-Scale
- Data Preprocessing
- [pre] Tokenization

### Prototypes
Since we employed a K-Medoid clustering for segmenting the papers, it is possible to characterize 4 distinct types of papers reported until 2019. 

| Influential Feature | Paper Type I |Paper Type II | Paper Type III |  Paper Type IV |
| --- | --- | --- | --- | --- | 
| SE Problem | Clone Detection | Software Testing | Software Security | Reliability & Defect Prediction |
| Architecture | RvNNs | Encoder-Decoder | FNN | Deep Belief Networks |
| Data-Scale | Thousands | Millions | Thousands | Tens |
| Data Preprocessing | Trees | Tokenization | Call Graph | Tokenization |
| [pre] Tokenization | Raw Code | Natural Language | non-Tokenization | Raw Code |


# Interpretation & Evaluation

## Statistical Facts and Arguments Supporting the SLR [2020 results]
### What types of SE tasks have been addressed by DL-based approaches?
- Arg1: Software Researchers use a diverse set of SE tasks when automating with DL approaches:
  - Fact 1: Around the 70% of the task distribution shows distinct SE tasks topics with percentages evenly distributed (~[3-5]%).
- Arg2: SE tasks is the most important feature from the entire study:
  - Fact 1: Self-information value is the highest with 4.031
  - Fact 2: It is one of the most influential features for grouping papers according to the centroids analysis.
  - Fact 3: 5 of 9 strong correlations (value >1 bit of mutual information) involves SE task feature.
### What types of Data are being used?
- Arg1: The main data employed in DL4SE papers are I/O Examples, Source Code, Natural Language, Repo Metadata and Vision.
  - Fact 1: Previous data represents the 78.57% of the distribution.
- Arg2: Execution traces and Bug reports are used in less proportion since both of them represents ~10% of the distribution. Which it is still a significant value.

### How is data being extracted and pre-processed into a format that the DL model can appropriately learn from?
- Arg1: The data-preprocessing feature contributes to decide about the architecture, loss function, and automation impact.
  - Fact 1: The self-information value of preprocessing is on average 0.5 bits distant from previous features.
- Arg2: The SE task and the venue determines the type of data preprocessing.
  - Fact 1: SE task and Venue have larger self-information values.
  - Fact 2: Both Se task and Venue have strong correlation with data preprocessing.
- Arg3: Neural Embeddings and Tokenizations are the most employed techniques in the Software Research community with around 57.14% of the distribution.

### What types of model architectures are used to perform automated feature engineering of the data related to various SE tasks?
- Arg1: The SE task influences the architecture to be employed in an approach.
  - Fact 1: The self-information value of SE task feature is higher than the architecture feature. 
  - Fact 2: The mutual information value between these two features is ~1.4
- Arg2: The software community focuses on textual-based problems since the most employed architecture is RNN used by the 40.48% of the papers. Additionally, some of the Encoder-Decoder approaches involves RNN computations. 
  - Fact 1: One of the most employed pre-processing techniques is the Tokenization used by the 27.38% of the papers.
  - Fact 2: The encoder-decoder architecture is used by 19% of the papers. 
- Arg3: Architecture is important for determining the type of paper given the centroid analysis.

### What learning algorithms and training processes are used in order to optimize the models?
- Arg1: The gradient descent is the most employed learning algorithm in all DL4SE approaches. However, a significant group in the community fail to report this feature. 
  - Fact 1: The learning algorithm feature has a stability of 78.57% which means that nearly all values are identical.
  - Fact 2: 13.10% of the papers do not report Learning-Algorithm.
- Arg2: The research community has been starting to employ learning algorithm different from GD in the recent years since reward policy and gradient ascent are found in papers published in 2018 and 2019. 
  - Fact 1: Around 6% of the papers report reward policy and gradient ascent in their approaches.
- Arg3: The loss function influences the learning algorithm with a mutual dependence of 0.72 bit. Nonetheless, the research community omit to report this parameter (around 27.38% of the papers). 
- Arg4: The majority of the reported software research ignores parameter optimization methods like grid search (around 54.76% of the papers do not employ Opt.Method).

### What baseline techniques are used to evaluate DL models and what metrics are analyzed for these comparisons?
- Arg1: For most of the papers that use Supervised learning, the authors omit to report ROC or AUC evaluations with a confidence level above ~0.87.

### How is the impact or automatization of DL approaches measured and in what way do these models promote generalizability?
- Arg 1: The Automation Impact is dependent on the SE task due to a high mutual information of 0.976 bits.
  - Fact 1: DL has increased Automation Efficiency across 75% of the SE Tasks.
  - Fact 2: 45.8% of SE tasks have seen advanced and novel architectures to solve.
  - Fact 3: DL has lead to replacement of lots of expertise through complete automation for 33% of SE tasks, especially Program Repair.

### What common factors contribute to the difficulty reproducing DL studies in SE?
- Arg 1: Irreproducibility of a paper is associated with the data used and the learning type of the DL approach.
  - Fact 1: Not using Vision and Repo Metadata, Natural Language, or I/O examples is associated with irreproducibility with strong support, above ~0.70, and confidence, above ~0.82.
  - Fact 2: Not using Vision, Repo Metadata, or I/O examples is associated with a lack of extraction details, which is an important step in reproducibility, with support above ~0.71 and confidence above ~0.82.
  - Fact 3: Not using Vision and Repo Meta based data is associated with a lack of data preprocessing details, which is an important step in reproducibility with support and confidence above ~0.74 and 0.81, respectively.
  - Fact 4: Using supervised learning is associated with a paper being irreproducible with a support and confidence of ~0.71 and ~0.87, respectively.


