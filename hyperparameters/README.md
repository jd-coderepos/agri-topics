# Hyper-parameters

This folder contains detailed records and analysis of the hyper-parameters used in our HDBSCAN clustering experiments aimed at developing a research topic taxonomy for Agriculture publications focused on irrigation. The README details each hyper-parameter's role in the clustering process, outlines the stability tests conducted, and discusses the correlation analysis performed to understand the relationships between hyper-parameters and clustering outcomes.

## UMAP Hyper-parameters

### `n_neighbors`

- **Description**: This parameter controls the size of the local neighborhood used for manifold approximation. It affects how UMAP balances local versus global structure in the data, with larger values promoting a broader view of the data manifold.
- **Typical Range**: Typically between 2 and 100, adjusted based on dataset characteristics and the desired scale of local to global structure.

### `n_components`

- **Description**: Specifies the number of dimensions to which the data should be reduced. Higher dimensions can capture more complex structures but may also introduce noise and computational complexity.
- **Default Value**: 2 (for visualization purposes), but can be set higher for more detailed analyses.

### `min_dist`

- **Description**: Controls how tightly UMAP allows points to cluster together, making an effective minimum distance between points in low-dimensional space. Smaller values result in tighter clusters, while larger values ensure points are more evenly distributed.
- **Typical Range**: Generally set relative to the `spread` parameter; often requires tuning based on the granularity desired.

### `umap_metrics`

- **Description**: The metric used to measure distances in the original space. The choice of metric can affect the shape and separation of clusters in the reduced space.

- **Options**: `euclidean`,  `cosine`

  

## HDBSCAN Hyper-parameters

### `min_cluster_size`

- **Description**: The minimum size of a cluster. Only clusters with at least this many samples are considered valid, while smaller groupings are treated as noise.
- **Typical Range**: Varies significantly with dataset size and density, generally starts from 5.

### `min_samples`

- **Description**: Determines the number of samples in a neighborhood for a point to be considered a core point. This affects the cluster's density requirement, with higher values leading to more strictly defined clusters.
- **Typical Range**: Usually between 1 and the number of features in the dataset, depending on desired cluster density.

### `cluster_selection_epsilon`

- **Description**: A distance threshold that causes HDBSCAN to merge smaller clusters into larger ones if the distance between clusters is below this value. Useful for controlling cluster granularity.
- **Typical Range**: Should be set based on the scale of the distance metric used; often requires tuning based on specific dataset characteristics.

### `cluster_selection_method`

- **Description**: Dictates the strategy used to select clusters from the condensed tree. Common methods include `eom` (Excess of Mass) and `leaf`, which influence how cluster hierarchies are simplified.
- **Options**: `eom`, `leaf`

### `hdbscan_metrics`

- **Description**: The metric used to calculate distances in feature space during clustering. Choice of metric can significantly affect the clustering outcome.
- **Options**: `euclidean`, `l2`



## Evaluation

### Correlation Analysis

The correlation analysis (pandas.DataFrame.corr(*method='pearson'*, *min_periods=1*, *numeric_only=False*)) between hyper-parameters and both `non_outlier_s_score` and `number_of_topics` reveals the following relationships:

- **UMAP `min_dist`**: This parameter, which determines the minimum separation between data points in reduced space, exhibits a moderate positive correlation with `non_outlier_s_score`. An increase in `min_dist` enhances the distinctiveness of clusters, suggesting that a greater separation helps in better defining the boundaries between different research topics. This finding is crucial as it highlights the balance needed between capturing local and global data structures to optimize topic extraction.
- **HDBSCAN `min_cluster_size` and `min_samples`**: Both these parameters show a moderate positive correlation with `non_outlier_s_score` and a strong negative correlation with `number_of_topics`. Increasing `min_cluster_size` and `min_samples` enhances the quality of the clusters but results in fewer distinct topics. This suggests that while the clustering becomes more robust and less noisy, the granularity of topic detection decreases.
- **HDBSCAN `cluster_selection_epsilon`**: This parameter shows negligible impact on both clustering quality and the number of topics detected, indicating its limited utility in this specific setup.
- **UMAP `n_components`**: There is a positive correlation with `number_of_topics`, implying that a higher dimension in the embedding space allows for a finer delineation of topics, potentially capturing more nuanced differences among them.
