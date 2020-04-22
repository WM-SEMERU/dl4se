# Data Mining Tasks for the SLR of DL4SE

> *Last update:* April 9th 2020; *Maintaned by* @danaderp

Before carrying out any mining tasks, we decided to address a basic descriptive statistics procedure for each feature. These statistics exhibit basic relative frequencies, counts, and quality metrics. We revised 4 quality metrics: ID-ness, Stability, Missing, and Text-ness. ID-ness measures to what extent a feature resembles an ID -the “title” is the only feature with a high ID-ness value. Stability measures to what extent a feature is constant (or have the same value). Missing is the number of missing values. Finally, Text-ness measures to what extent a feature contains free-text. The results for each feature can be navigated once the SRL process is run in RapidMiner. As for the data mining tasks employed, we focused on three of them: Correlations, Association Rule Learning, and Clustering.

## Correlations by Mutual Information
Due to the nominal nature of the SLR features, we were unable to perform classic statistical correlations on our data (i.e. pearson’s correlation). However, we adapted an operator based on attributes’ information dependencies. This operator is known as “mutual information” [(MacKay, 2005)](https://www.inference.org.uk/itprnn/book.pdf). Similar to covariance or pearson’s correlation, we were able to represent the outcomes of the operator in a confusion matrix (for further details please run the RapidMiner process).

![Mutual Information Matrix of DL4SE](https://github.com/WM-CSCI-435-F19/dl4se/blob/master/results/correlation/ConfusionMatrixMutualInformation.png)

The mutual information measures to what extent one feature knows about another one. High mutual information values represent less uncertainty; therefore, we built arguments like “the deep learning architecture used on a paper is less uncertain (or more predictable) given a particular SE task” or  “the reported architecture on the papers are mutually dependent with the Software Engineering task”. 

The difference between correlations and associations rules depends on the granularity level of the data (e.g., paper, feature, or class). The correlation procedure was performed at the feature level, while the association rule learning was performed at the class or category level.

### Association Rule Learning
For generating the association rules, we employed the classic FP-Growth algorithm (FP stands for Frequent Pattern). The FP-Growth computed frequently co-occurring items in our transactional dataset (see DL4SE-Dataset.csv) with a min support of 0.95. These items comprise each class per feature. For instance, the feature “Loss Function” exhibits a set of classes (or items) such as “NCE”, “Max Log Likelihood”, “Cross-Entropy”, “MSE”, “Hinge Loss”, “N/A-Loss”, and so on. The main premise of the FP-Growth can be summarized as: if an itemset is frequent (i.e. {MSE, RNN}),  then all of its item subsets are also frequent (i.e. {MSE} and {RNN}).

Once the FP-Growth generated the itemsets (e.g. {MSE, Hinge Loss}), the algorithm started to check the itemsets’ support in continuous scans of the database. Such support measures the occurances of the itemset in the database. The FP-Growth scans on the DB brought about a FP-tree data structure. We recursively mined the FP-tree to extract frequent itemsets [(Han, et al; 2000)](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.40.4436). Nonetheless, the association rule learning requires more than the FP-tree extraction.

![Association Rule Learning of DL4SE](https://github.com/WM-CSCI-435-F19/dl4se/blob/master/results/association/association_rules.png)

An association rule serves as an if-then statement (or premise-conclusion) based on the frequent itemset pattern. Let’s observe the following rule mined from our dataset: “Given that an author used Supervised Learning, we can conclude that their approach is irreproducible with a support of 0.7 and a confidence of 0.8”. We observe the association rule has an antecedent (i.e., the itemset {Supervised Learning}) and a consequent (i.e.,the itemset {Irreproducible}). These relationships are mined from itemsets that usually have a high support and confidence. The confidence is a measure of the number of times that premise-conclusion statement is found to be true. We tuned both parameters to be greater than 0.7.

We refute the idea that association rule learning avoids spurious correlations. Nonetheless, we organized the rules into an interconnected net of premises/conclusions to find explanations around techniques and methodologies reported on the papers. Any non-logical rule is disregarded as well as rules that possess lower support and confidence. 

### Clustering
Clustering operators group or segment instances of data that are similar to each other and -at the same time- differentiable from other instances in other clusters. Additionally, this mining task describes datasets that are unlabeled by identifying a prototype or an instance that represents the entire cluster. For our EDA, we carried out a segmentation of papers using the k-medoids algorithm [(Kaufman & Rousseeuw; 1987)](https://wis.kuleuven.be/stat/robust/papers/publications-1987/kaufmanrousseeuw-clusteringbymedoids-l1norm-1987.pdf). We wanted to make sure that the centroids of the generated clusters are actual points (papers) from the dataset. 

We built two distinct pipelines in RapidMiner for the paper grouping: 1) a PCA clustering and 2) an explainable clustering. On the one hand, the goal of the first pipeline is to visualize the points (or papers) in a reduced dimensionality (two pcs components). The PCA clustering allowed us to find the maximum change in variation when testing different k clusters values. The PCA clustering shows that 4 clusters were ideal for the analysis. On the other hand,  the goal of the second pipeline is to identify which features are influencing the segmentation. Knowing these features allowed us to explain how the algorithm was segmenting the space. Finally, we provided the descriptions of four types of papers found during the clustering analysis.  

