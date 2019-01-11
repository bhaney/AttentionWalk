Attention Walk
============================================
A PyTorch Implementation of "Watch Your Step: Learning Node Embeddings via Graph Attention" (NIPS 2018).
<div style="text-align:center"><img src ="attentionwalk.jpg" ,width=720/></div>
<p align="justify">
Graph embedding methods represent nodes in a continuous vector space, preserving different types of relational information from the graph. There are many hyper-parameters to these methods (e.g. the length of a random walk) which have to be manually tuned for every graph. In this paper, we replace previously fixed hyper-parameters with trainable ones that we automatically learn via backpropagation. In particular, we propose a novel attention model on the power series of the transition matrix, which guides the random walk to optimize an upstream objective. Unlike previous approaches to attention models, the method that we propose utilizes attention parameters exclusively on the data itself (e.g. on the random walk), and are not used by the model for inference. We experiment on link prediction tasks, as we aim to produce embeddings that best-preserve the graph structure, generalizing to unseen information. We improve state-of-the-art results on a comprehensive suite of real-world graph datasets including social, collaboration, and biological networks, where we observe that our graph attention model can reduce the error by up to 20%-40%. We show that our automatically-learned attention parameters can vary significantly per graph, and correspond to the optimal choice of hyper-parameter if we manually tune existing methods.</p>

This repository provides an implementation of Attention Walk as described in the paper:

> Watch Your Step: Learning Node Embeddings via Graph Attention.
> Sami Abu-El-Haija, Bryan Perozzi, Rami Al-Rfou, Alexander A. Alemi.
> NIPS, 2018.
> [[Paper]](http://papers.nips.cc/paper/8131-watch-your-step-learning-node-embeddings-via-graph-attention)

### Requirements

The codebase is implemented in Python 3.5.2. package versions used for development are just below.
```
networkx          1.11
tqdm              4.28.1
numpy             1.15.4
pandas            0.23.4
texttable         1.5.0
scipy             1.1.0
argparse          1.1.0
torch             1.0.0.
torchvision       0.2.1
```
### Datasets

The code takes an input graph in a csv file. Every row indicates an edge between two nodes separated by a comma. The first row is a header. Nodes should be indexed starting with 0. Sample graphs for the `Twitch Brasilians` and `Wikipedia Chameleons` are included in the  `input/` directory. 

### Options

Learning of the embedding is handled by the `src/main.py` script which provides the following command line arguments.

#### Input and output options

```
  --edge-path    STR     Input graph path.           Default is `input/chameleon_edges.csv`.
  --output-path  STR     Embedding path.             Default is `output/chameleon_attention_walk.csv`.
```

#### Model options

```
  --dimensions              INT       Number of embeding dimensions.         Default is 128.
  --budget                  INT       Sampling budget.                       Default is 10^5.
  --node-noise-samples      INT       Number of noise sampled nodes.         Default is 5.
  --feature-noise-samples   INT       Number of noise sampled features.      Default is 5.
  --batch-size              INT       Number of source nodes per batch.      Default is 32.
  --walk-length             INT       Truncated random walk length.          Default is 80.  
  --number-of-walks         INT       Number of walks per source node.       Default is 10.
  --window-size             INT       Skip-gram window size.                 Default is 5.
  --learning-rate           FLOAT     Learning rate value.                   Default is 0.001.
```

### Examples

The following commands learn a graph embedding and write the embedding to disk. The node representations are ordered by the ID.

Creating a SINE embedding of the default dataset with the default hyperparameter settings. Saving the embedding at the default path.

```
python src/main.py
```
<p align="center">
<img style="float: center;" src="sine_run_example.jpg">
</p>

Creating a SINE embedding of the default dataset with 256 dimensions.

```
python src/main.py --dimensions 256
```

Creating a SINE embedding of the default dataset with a low sampling budget.

```
python src/main.py --budget 1000
```

Creating an embedding of an other dense structured dataset the `Twitch Brasilians`. Saving the output in a custom folder.

```
python src/main.py --edge-path input/ptbr_edges.csv --feature-path input/ptbr_features.json --output-path output/ptbr_sine.csv
```

