# SUSHI🍣
SUSHI🍣: A Dataset of Synthetic Unichannel Signals Based on Heuristic Implementation

## Overview
This dataset consists of pairs of time series signal data and corresponding natural language texts that describe the characteristics of these time series patterns. It has been designed with the objective of creating and evaluating a foundational model that facilitates natural language processing tasks, such as query-by-text retrieval for time series signals and captioning for these signals. All time series signals included in this dataset have been artificially generated from predefined combinations of multiple classes of functions. In addition, the paired natural language texts are primarily based on texts randomly selected from a pre-registered list corresponding to the functions, which have been manually refined through visual inspection.

## Specification
| Spec | Tiny | Base |
| :--- | :--- | :--- |
| Samples | 1.4K | 140K |
| Time length | 2048 points | 2048 points |
| Format | CSV, NPY, PNG | CSV, NPY, PNG |

## Weakness of Conventional Datasets
There are no existing datasets that cater to this specific objective. 
1. Firstly, although some datasets are available in the field of chart summarization, most of them have ambiguous rights, making them unsuitable for commercial use.
2. Secondly, the descriptions in the texts of these datasets depend on the context inferred from the characters initially included in the chart rather than on the characteristics of the time series itself.

To our knowledge, TRUCE [^1] is the only dataset that somewhat aligns with our objective; however, it has its own set of issues.

3. The time series of TRUCE is stock prices, so an imbalance of the time series patterns appears.
4. The time length of the signal is short, with only 12 points.
5. The quality of the text description is not high due to a limitation on the number of characters.

[^1]: Harsh Jhamtani and Taylor Berg-Kirkpatrick, "Truth-Conditional Captioning of Time Series Data," in Proc. Conference on Empirical Methods in Natural Language Processing (EMNLP), 2021, pp. 719–733. [URL](https://aclanthology.org/2021.emnlp-main.55.pdf)

## Strength of the SUSHI Dataset
1. When we created this dataset, the time series signals were simply compositions of functions predefined by formulas, and the texts were based on ideas we manually conceived, thereby eliminating any copyright issues with the training data.
2. Unlike those in existing datasets, the texts included in this dataset do not contain any context-dependent descriptions and only describe the features of the time series signals themselves.
3. Unlike existing data that is umbalanced towards specific domain time series patterns, the functions we've incorporated, based on our experience analyzing industrial sensor data, cover time series patterns we believe to be common.
4. The signal's length is 2048 points, which is equivalent to the length of the time window in many sensor data analyses.
5. We have visually confirmed that all data is accompanied by accurate captions, albeit somewhat redundant, ensuring a high quality of captions.

## Method for Making Dataset
### 1. Class definition
Referring to Imani, et al. [^2], we defined the trends as Constant, Linear increase, Linear decrease, Concave, Convex, and the fluctuations as Noisy, Positive spiky, Negative spiky, Positive-and-negative spiky, and Steppy.
Additionally, based on our experience, we added frequently observed time-series patterns in time-series data analysis.
Please refer to the list of the defined classes below.

[^2]: S. Imani, S. Alaee, and E. Keogh, "Putting the human in the time series analytics loop," in Proc. World Wide Web Conference (WWW), 2019, pp. 635–644. [URL](https://doi.org/10.1145/3308560.3317308)

### 2. Synthesis
For each sample, we repeatd the following process (i.e., different parameters were randomly selected for each sample):
1. We randomly generated the parameters of $x_{tr}(t)$ corresponding to the trend function (for example, $a$ and $b$ in the case of Linear increase), and under these parameters, we generated the time series signal $x_{tr}(t)$.
2. We randomly generated the parameters of $x_{pe}(t)$ corresponding to the periodic function (for example, $a$, $b$, $\lambda$, and $\phi$ in the case of a Sinusoidal wave), and under these parameters, we generated the time series signal $x_{pe}(t)$.
3. We randomly generated the parameters of $x_{fl}(t)$ corresponding to the fluctuation function (for example, $\sigma$ of Gaussian noise), and under these parameters, we generated the time series signal $x_{fl}(t)$.
4. Next, we synthesized the final time series signal $y(t)$ as follows:
```math
y(t) = x_{tr}(t) + x_{pe}(t) + x_{fl}(t)
```
Note that in the current version, the trend and periodic functions are mutually exclusive, and we limited to either of the following patterns (we plan to upgrade to the above equation in the future):
```math
y(t) = x_{tr}(t) + x_{fl}(t)
```
or
```math
y(t) = x_{pe}(t) + x_{fl}(t)
```

## Classes of functions
### Trend functions

| Class | Formura | An Example of Captions |
| :--- | :--- | :--- |
| Constant | $x_{tr}(t) = a$  | "The trend remains constant, with no change from a specific value." |
| Linear increase | $x_{tr}(t) = a t + b \quad (a > 0)$  | "The trend is linearly increasing at a constant rate." |
| Linear decrease | $x_{tr}(t) = a t + b \quad (a < 0)$  | "The trend is linearly decreasing at a constant rate." |
| Concave | $x_{tr}(t) = a t^2 + b t + c \quad (a < 0)$  | "The trend follows a concave curve, similar to a quadratic function, which initially ascends and then descends." |
| Convex | $x_{tr}(t) = a t^2 + b t + c \quad (a > 0)$  | "The trend follows a convex curve, similar to a quadratic function, which initially descends and then ascends." |
| Exponential growth | $x_{tr}(t) = a \exp(b t) + c \quad (a > 0, b > 0)$ | "The trend is characterized by exponential growth, culminating in a sharp rise towards the end." |
| Inverted exponential growth | $x_{tr}(t) = a \exp(b t) + c \quad (a < 0, b > 0)$ | "The trend exhibits a decreasing inverted exponential growth, characterized by a sharp decline at the end." |
| Exponential decay | $x_{tr}(t) = a \exp(b t) + c \quad (a > 0, b < 0)$ | "The trend is characterized by a decrease, converging to a certain value through exponential decay." |
| Inverted exponential decay | $x_{tr}(t) = a \exp(b t) + c \quad (a < 0, b < 0)$ | "The trend is approaching saturation through an increase in inverted exponential decay." |
| Sigmoid | $x_{tr}(t) = a / (1 + \exp(-b(t - c))) + d \quad (a > 0, b > 0)$ | "The trend shows a monotonically increasing pattern, like a sigmoid curve that forms an S-shape, eventually converging to a certain value, with its rate of increase gradually slowing down." |
| Inverted sigmoid | $x_{tr}(t) = a / (1 + \exp(-b(t - c))) + d \quad (a < 0, b > 0)$ | "The trend shows a monotonically decreasing pattern, like an inverted sigmoid curve that forms a mirrored S-shape, eventually converging to a certain value, with its rate of decrease gradually slowing down." |
| Cubic function | $x_{tr}(t) = a t^3 + b t^2 + c t + d \quad (a > 0)$ | "The trend resembles the curve of a positive cubic function, initially increasing, then decreasing in the middle, and finally turning back to increasing. This forms a mirrored 90-degree rotated S-shape." |
| Negative cubic function | $x_{tr}(t) = a t^3 + b t^2 + c t + d \quad (a < 0)$ | "The trend resembles the curve of a negative cubic function, initially decreasing, then increasing in the middle, and finally turning back to decreasing. This forms a 90-degree rotated S-shape." |
| Gaussian | $x_{tr}(t) = a \exp (- ((t - b) / c)^2) + d \quad (a > 0)$ | "The trend follows the curve of a Gaussian function, initially ascending and then descending, with both ends ultimately converging to a constant value." |
| Inverted Gaussian | $x_{tr}(t) = a \exp (- ((t - b) / c)^2) + d \quad (a < 0)$ | "The trend follows the curve of an inverted Gaussian function, initially descending and then ascending, with both ends ultimately converging to a constant value." |

### Periodic functions

| Class | Formura | An Example of Captions |
| :--- | :--- | :--- |
| Sinusoidal wave | $x_{pe} = a \sin(2 \pi t / \lambda + \phi) + b$ | "The signal is showing a periodic pattern that repeats at regular intervals, like a sinusoidal wave." |
| Square wave | $x_{pe} = a \mbox{sgn}( \sin(2 \pi t / \lambda + \phi) ) + b$ | "The signal is showing a periodic pattern that repeats at regular intervals, like a square wave." |
| Sawtooth wave | $x_{pe} = a \mbox{sawtooth}(t / \lambda + \phi) + b$<BR>$(a > 0, \mbox{sawtooth}(t) = 2 (t - \mbox{floor}(t + 1/2)))$ | "The signal is showing a periodic pattern that repeats at regular intervals, like a sawtooth wave." |
| Reverse sawtooth wave | $x_{pe} = a \mbox{sawtooth}(t / \lambda + \phi) + b$<BR>$(a < 0, \mbox{sawtooth}(t) = 2 (t - \mbox{floor}(t + 1/2)))$ | "The signal is showing a periodic pattern that repeats at regular intervals, like a reverse sawtooth wave." |
| Triangle wave | $x_{pe} = a \mbox{triangle}(t / \lambda + \phi) + b$<BR>$(\mbox{triangle}(t) = 2 \|2 (t - \mbox{floor}(t + 1/2))\| - 1)$ | "The signal is showing a periodic pattern that repeats at regular intervals, like a triangle wave." |

### Fluctuations
| Class | Gaussian noise | Positive spikes | Negative spikes | Step changes | An Example of Captions |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Clean | No | No | No | No | - |
| Smooth | small | small | small | small | "The signal is generally smooth with only minor noise and spikes." |
| Noisy | large | small | small | small | "There is a large amount of noise throughout the signal." |
| Positive spiky | small | large | small | small | "There are large positive spikes throughout the signal." |
| Negative spiky | small | small | large | small | "There are large negative spikes throughout the signal." |
| Positive-and-negative spiky | small | large | large | small | "There are large positive and negative spikes throughout the signal." |
| Steppy | small | small | small | large | "There are large step changes throughout the signal." |
