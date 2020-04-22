# Correlations, Associations, and Clustering Results
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
![Clustering Description](https://wm-csci-435-f19.github.io/dl4se/results/clustering/%5Bclustering%5D%20PCA-Clustering.PNG)
### Clustering visualization
![PCA Clustering](https://wm-csci-435-f19.github.io/dl4se/results/clustering/%5Bclustering%5D%20PCA-Dimension.PNG)
## Explainable Clustering
Using PCA for clustering fails to provide explainable features for clustering decision. Therefore, we used all features witout any trasformation and then perform K-Meoids algorithm with nominal measures and Kulczynski Similarity. The following plot depicts the cetroids values to determine which features contributes the most for segmenting the papers.
![Centroids Plot](https://wm-csci-435-f19.github.io/dl4se/results/clustering/%5Bclustering%5D%20Centroids.PNG)
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


# Interpretation & Evaluation

## Statistical Facts and Arguments Supporting the SLR 
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


