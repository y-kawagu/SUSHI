# SUSHI🍣
SUSHI🍣: A Dataset of Synthetic Uni-Channel Signals Based on Heuristic Implementation

## Overview
This dataset consists of pairs of time series signal data and corresponding natural language texts that describe the characteristics of these time series patterns. It has been designed with the objective of creating and evaluating a foundational model that facilitates natural language processing tasks, such as query-by-text retrieval for time series signals and captioning for these signals. All time series signals included in this dataset have been artificially generated from predefined combinations of multiple classes of functions. In addition, the paired natural language texts are primarily based on texts randomly selected from a pre-registered list corresponding to the functions, which have been manually refined through visual inspection.

## Weakness of Conventional Datasets
There are no existing datasets that cater to this specific objective. 
1. Firstly, although some datasets are available in the field of chart summarization, most of them have ambiguous rights, making them unsuitable for commercial use.
2. Secondly, the descriptions in the texts of these datasets depend on the context inferred from the characters initially included in the chart rather than on the characteristics of the time series itself.

To our knowledge, TRUCE [^1] is the only dataset that somewhat aligns with our objective; however, it has its own set of issues.

3. The time series of TRUCE is stock prices, so an imbalance of the time series patterns appears.
4. The time length of the signal is short, with only 12 points.
5. The quality of the text description is not high due to a limitation on the number of characters.

[^1]: Harsh Jhamtani and Taylor Berg-Kirkpatrick, "Truth-Conditional Captioning of Time Series Data," in EMNLP, 2021.  [URL](https://aclanthology.org/2021.emnlp-main.55.pdf)

## Strength of the SUSHI Dataset
1. When we created this dataset, the time series signals were simply compositions of functions predefined by formulas, and the texts were based on ideas we manually conceived, thereby eliminating any copyright issues with the training data.
2. Unlike those in existing datasets, the texts included in this dataset do not contain any context-dependent descriptions and only describe the features of the time series signals themselves.
3. Unlike existing data that is umbalanced towards specific domain time series patterns, the functions we've incorporated, based on our experience analyzing industrial sensor data, cover time series patterns we believe to be common.
4. The signal's length is 2048 points, which is equivalent to the length of the time window in many sensor data analyses.
5. We have visually confirmed that all data is accompanied by accurate captions, albeit somewhat redundant, ensuring a high quality of captions.

## Method for Making Dataset

## Classes of functions
### Trend functions
### Periodic functions
### Fluctuations
