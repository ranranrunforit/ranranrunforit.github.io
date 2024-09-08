---
title: "Reproducibility Project for Graph Attention Networks"
excerpt: "Graph Attention Networks using Python PyTorch.<br/><img src='/images/GAT.png'>"
collection: projects
date: 2024-05-06
tags:
  - PyTorch
  - PyTorch Geometric
  - NumPy
  - scikit-learn
  - Ipython
  - matplotlib
---

The publication: *[P. VelickoviÊ, G. Cucurull, A. Casanova, A. Romero, P. Lio, and Y. Bengio, “Graph attention networks,” arXiv preprint arXiv:1710.10903, 2017.](https://doi.org/10.48550/arXiv.1710.10903)*  


The paper introduces Graph Attention Networks (GAT) as an innovative approach to processing graph-structured data using convolution-style neural networks with masked self-attentional layers. GAT enables nodes to assign varying weights to their neighbors without costly matrix operations or prior knowledge of the entire graph structure. By leveraging attention mechanisms efficiently within the graph attentional layer, GAT achieves computational efficiency and can parallelize operations across all nodes in the graph. This dynamic assignment of node importance based on relationships leads to improved information propagation and feature learning, resulting in state-of-the-art performance on node classification benchmarks. GAT offers a more effective and scalable solution for handling complex relationships in graph data.



**[Project Demo](https://www.youtube.com/watch?v=QZgEJTrxImE)**


# Dataset

The project utilized three classic graph datasets, namely Cora, CiteSeer, and Protein-Protein Interactions (PPI) datasets.

For transductive learning, both Cora and CiteSeer Datasets were used, representing networks of research papers with each connection representing a citation. The Cora dataset consists of 2708 scientific publications classified into seven classes, with a citation network of 5429 links. Each publication is described by a word vector (node features) indicating the presence or absence of 1433 unique words in a paper. The CiteSeer dataset consists of 3327 scientific publications classified into six classes, with a citation network of 4732 links. Each publication is described by a word vector indicating the presence or absence of 3703 unique words.

For inductive learning, the PPI dataset contains 24 graphs corresponding to different human tissues, with 20 graphs for training, 2 for validation, and 2 for testing. The average number of nodes per graph is 2372, and each node has 50 features composed of positional gene sets, motif gene sets, and immunological signatures, as well as 121 labels from the Molecular Signatures Database.

# Model Architecture

The model architecture for Graph Attention Networks (GAT) as proposed by Veličković et al. consists of two main model configurations, transductive learning model on Cora and Citeseer datasets, and inductive learning model on the PPI dataset. Both models are initialized using Glorot initialization and trained to minimize cross-entropy on the training nodes using the Adam SGD optimizer with an initial learning rate of 0.005 for all datasets. In both cases, an early stopping strategy is employed on both the cross-entropy loss and accuracy (transductive) or micro-F1 (inductive) score on the validation nodes, with a patience of 100 epochs.

For transductive learning, a two-layer GAT model is applied. The first layer consists of K = 8 attention heads computing F = 8 features each, followed by an exponential linear unit (ELU) nonlinearity. This results in a total of 64 features. The second layer is used for classification and consists of a single attention head that computes C features, where C is the number of classes, followed by a softmax activation. To address the small training set sizes, regularization is applied within the model. Specifically, L2 regularization with lambda = 0.0005 is applied during training. Additionally, dropout with p = 0.6 is applied to both layers’ inputs, as well as to the normalized attention coefficients.
The architectural hyperparameters have been optimized on the Cora dataset and then reused for the Citeseer dataset.

For inductive learning, a three-layer GAT model is applied. Both of the first two layers consist of K = 4 attention heads computing F = 256 features each, followed by an ELU nonlinearity, resulting in a total of 1024 features. The final layer is used for (multi-label) classification and consists of K = 6 attention heads computing 121 features each, which are averaged and followed by a logistic sigmoid activation. In this case, the training sets are sufficiently large, and there is no need to apply L2 regularization or dropout. However, skip connections across the intermediate attentional layer have been successfully employed. A batch size of 2 graphs is utilized during training for this task.

# Results

The model performance is shown in the below table.

|  Info \ Dataset |         Cora        |       Citeseer      |        PPI        |
| --------------- | ------------------- | ------------------- | ----------------- |
|       Task      |     Transductive    |     Transductive    |     Inductive     |
|       GCN       |   0.8037 ± 0.0109   |   0.7005 ± 0.0030   |        N/A        |
|       GAT       | **0.8285 ± 0.0037** | **0.7293 ± 0.0057** |**0.9751 ± 0.0016**|


In transductive learning, the GAT model was trained for 200 epochs over 10 runs using default hyperparameters on randomly split train/val/test data. The model achieved approximately 82.85% ± 0.57% accuracy on the test split of the Cora Dataset and 72.93% ± 0.37% accuracy on the test split of the Citeseer Dataset. These results are comparable to the performance reported in the original paper, supporting hypotheses 1 and 2. The variability in results can be attributed to the randomness of the train/val/test split.

In the case of inductive learning, after 10 runs of training for 200 epochs, the GAT model achieved an average of 97.51% ± 0.16% micro F1 score on the test split.

# Reference

``` 
@article{
  velickovic2018graph,
  title="{Graph Attention Networks}",
  author={Veli{\v{c}}kovi{\'{c}}, Petar and Cucurull, Guillem and Casanova, Arantxa and Romero, Adriana and Li{\`{o}}, Pietro and Bengio, Yoshua},
  journal={International Conference on Learning Representations},
  year={2018},
  url={https://openreview.net/forum?id=rJXMpikCZ},
}
```


