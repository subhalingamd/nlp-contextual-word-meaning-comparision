# Context-Sensitive Word Sense Disambiguation 

<!-- MarkdownTOC -->

1. [Motivation](#motivation)
1. [Problem Statement](#problem-statement)
1. [Dataset](#dataset)
1. [Evaluation](#evaluation)
1. [Methodology](#methodology)
	1. [BiLSTM + GloVe](#bilstm--glove)
	1. [BERT](#bert)
1. [Running the code](#running-the-code)
1. [Results](#results)

<!-- /MarkdownTOC -->


<a id="motivation"></a>
## Motivation
Words often have multiple meanings associated with them and the surrounding context often determines the specific meaning of the word which is used. The goal of this assignment is to develop deep neural models that can identify whether a particular word used in a sentence pair has the same meaning in both sentences or has a different meaning in each of the sentences.

For example,
```
S1: We often used to play on the [bank] of the river.
S2: We lived along the [bank] of the Ganges.
S3: He cashed a check at the [bank].
```
S1 and S2 use the same meaning of the word bank (*river bed*) while S1 and S3 use different meanings of the word bank (*river bed* vs. *financial institution*).

<a id="problem-statement"></a>
## Problem Statement

We frame the task as a classification problem, in which the input sentence pair (*X*) and the word (*W*) must be classified into a label **T** if *W* has the same meaning in both sentences of *X* and **F** if *W* has different meanings.
 
*X*, *W*            | Ground Truth Label
:------------------:|:------------------:
(*S1*, *S2*), bank  | **T**
(*S1*, *S3*), bank  | **F**
(*S2*, *S3*), bank  | **F**


<a id="dataset"></a>
## Dataset
**Dataset used:** WiC: The Word-in-Context Dataset (English) [[Link]](https://pilehvar.github.io/wic/)

The `*.data.txt` correspond to the input file and `*.gold.txt` correspond to the final labels. Further details on the format can be found from the link mentioned above.

<a id="evaluation"></a>
## Evaluation
The performance on the task will be evaluated using **binary accuracy**.

<a id="methodology"></a>
## Methodology
This assignment is divided into two parts:

<a id="bilstm--glove"></a>
### BiLSTM + GloVe
In the first part, we build a baseline without using contextual embeddings, pretrained models, external knowledge, additional datasets, etc. The goal is to understand how contextual embeddings are important in NLP.

For this, we make use of GloVe embeddings and build BiLSTM model. The hyperparameters can be found directly from `args` in the notebook.

<a id="bert"></a>
### BERT
In this part, we do not have any restrictions, *other than constraints on availability of computational resources*.

We use BERT in this part. More specifically, we use [`BertForSequenceClassification`](https://huggingface.co/docs/transformers/model_doc/bert) from [Hugging Face's Transformers](https://huggingface.co/docs/transformers/index) library and start finetuning from [`bert-base-cased`](https://huggingface.co/bert-base-cased) checkpoint.

Each data-point is encoded as `[CLS] sentence1 [SEP] sentence2 [SEP]` (the *target word*, *indices*, *POS* info are not used) which is classified into 0/1. `AdamW` is used for optimization. The hyperparameters can be found directly from `args` in the notebook.

Further, the dataset is augmented with (subset of) [MCL-WiC dataset from SemEval 2021 Task 2](https://github.com/SapienzaNLP/mcl-wic) while finetuning.

<a id="running-the-code"></a>
## Running the code
This assignment uses Python Notebooks. The files organization is given below:
```
    |-- _data/             : original dataset
    |-- _add_data/         : additional data used
    |-- A__BiLSTM_GloVe/   : training and inference notebooks for part 1
    |-- B__BERT/           : training and inference notebooks for part 2
    |-- README.md              
```
 If you are planning to run the code by yourself, you might want to make changes in the first and last (few) cell(s) and the file paths in `args`. Following this, you can click on *Run All* and run all the cells.


<a id="results"></a>
## Results
Model                                 | Val. Acc. | Test Acc. 
--------------------------------------|:---------:|:---------:
A. BiLSTM + GloVe                     | 58.93     | 54.57	 
B. BERT <sup>(w/ augmentation)</sup>  |	72.10     | 68.43



----
*This README uses texts from the assignment problem document provided in the course.*