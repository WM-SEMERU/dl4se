## Interpretation & Evaluation
>
> This analysis was performed by @danaderp and @ncoop57
>

# Correlations by Mutual Information
The following relationships are strong correlated by mutual information. It shows us how much one attribute tells us about another one. A high mutual information means a large reduction of uncertainty. 
- SE Task ⟷ Data Preprocessing [1.8]
- SE Task ⟷ Venue (1.76)
- SE Task ⟷ Loss Function (1.446)
- SE Task ⟷ Architecture (1.397)
- SE Task ⟷ Other SE Data (1.107)
- Venue ⟷ Data Preprocessing (1.292) 
- Tuning ⟷ Opt. Method (1.248) 
- Loss Function ⟷ Data Preprocessing (1.133)
- Other SE Data ⟷ Data Preprocessing (1.006)

## Self-Information (or Entropy-based) features

| Most Informative Features | Self-Inf Value| Less Informative Features | Self-Inf. Value|
| --- | --- | --- | --- | 
| SE-Task | 4.031 | Roc&AUC | 0.371 |
| Venue | 3.349 | BLEU Score | 0.371 |
| Data Preprocessing | 3.183 | Vision | 0.414 |
| Loss Function | 3.027 | MRR | 0.414 |
| Architecture | 2.696 | I/O Examples | 0.527 |

# Association Rules Reasoning
1. **A reproducibility argument**: For most of the papers that exclude vision and repo meta, input/output examples, or natural language based data, the authors fail to provide enough data to reproduce their approach with confidence levels above ~0.82.
2. **A learning type argument**: For most of the papers that use Supervised learning, the authors omit to report ROC or AUC evaluations, data extraction details, and are irreproducible with confidence levels above ~0.87.
3. **A preprocessing argument**: In the papers that exclude vision or repo meta based data, most of the authors fail to report how they preprocessed their data with confidence levels above ~0.81.
4. **The data extraction argument**: Many of the papers that exclude input/output examples, vision, natural language, or repo meta based data omit how their data was extracted with confidence levels above ~0.82.
5. **A learning algorithm argument**: For most of the papers that exclude natural language or repo meta based data, the authors fail to discuss the learning algorithm employed with confidence levels around ~0.82. 

# Clustering
## K-means and PCA Reduction
![Clustering Description](https://github.com/WM-CSCI-435-F19/dl4se/blob/master/results/clustering/%5Bclustering%5D%20PCA-Clustering.PNG)
### Clustering visualization
![PCA Clustering](https://github.com/WM-CSCI-435-F19/dl4se/blob/master/results/clustering/%5Bclustering%5D%20PCA-Dimension.PNG)
## Explainable Clustering
Using PCA for clustering fails to provide explainable features for clustering decision. Therefore, we used all features witout any trasformation and then perform K-Meoids algorithm with nominal measures and Kulczynski Similarity. The following plot depicts the cetroids values to determine which features contributes the most for segmenting the papers.
![Centroids Plot] (https://github.com/WM-CSCI-435-F19/dl4se/blob/master/results/clustering/%5Bclustering%5D%20Centroids.PNG)
### Influential Features
The following features are contributing the most for segmenting papers. You can observe in the previous plot the distance exhibit for each feature respect to the centroid value. 
- Venue
- Loss Function
- SE-Task
- Data Preprocessing
- Architecture
- Automation Impact
### Prototypes
Since we employed a K-Medoid clustering for segmenting the papers, it is possible to characterize 4 distinct types of papers reported until 2019. 

| Influential Feature | Paper Type I |Paper Type II | Paper Type III |  Paper Type IV |
| --- | --- | --- | --- | --- | 
| Venue | TSE | TSE | neuroIPs | ArXiv |
| Loss Function | Max Log Likelihood | MSE | MAPO | Cross-Entropy |
| SE Task | Retrieval-Trace | Image2Structure | Program Synthesis | Source Code Generation |
| Data Preprocessing | Neural Embedding | Neural Embedding | Directed Graph | BPE |
| Architecture | Encoder-Decoder | CNN | Encoder-Decoder | RNN |
| Automation Impact | Replacing Expertise | Automation/Efficiency | Advanced Architecture/Novelty | Open Vocabulary Issue |




