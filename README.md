# A Systematic Literature Review of Deep Learning in Software Engineering

> *Last update:* April 9th 2020; *Maintaned by* @danaderp
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
