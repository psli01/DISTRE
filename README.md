# Fine-tuning Pre-Trained Transformer Language Models to Distantly Supervised Relation Extraction

This repository contains the code of our paper:  
[Fine-tuning Pre-Trained Transformer Language Models to Distantly Supervised Relation Extraction](https://www.aclweb.org/anthology/P19-1134)  
Christoph Alt, Marc Hübner, Leonhard Hennig


Our code depends on huggingface's [PyTorch reimplementation](https://github.com/huggingface/pytorch-openai-transformer-lm) of the [OpenAI GPT](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf), and [AllenNLP](https://allennlp.org/) - so thanks to them.

The code is tested with:
- Python 3.6.6
- PyTorch 1.0.1
- AllenNLP 0.7.1

## Installation

First, clone the repository to your machine and install the requirements with the following command:

```bash
pip install -r requirements.txt
```

Second, download the [OpenAI GPT archive](https://cloud.dfki.de/owncloud/index.php/s/kKdpoaGikWnL4tn/download) (containing all model related files):
```
wget --content-disposition https://cloud.dfki.de/owncloud/index.php/s/kKdpoaGikWnL4tn/download
```

## Prepare the data
We evaluate our model on the [NYT dataset](http://www.riedelcastro.org//publications/papers/riedel10modeling.pdf) and use the version provided by [OpenNRE](https://github.com/thunlp/OpenNRE).

Follow the OpenNRE instructions for creating the NYT dataset in JSON format:

1) download the [nyt.tar file](https://github.com/thunlp/OpenNRE#provided-data).
2) change the file name as nyt.tar
3) extract the archive with: `tar -xvf nyt.tar`
4) convert the protobuf files to json: `python protobuf2json.py .`
5) move `train.json` and `test.json` to `data/open_nre_nyt/`

## Training
E.g. for training on the NYT dataset, run the following command:

```bash
CUDA_VISIBLE_DEVICES=0 allennlp train \
    experiments/configs/model_paper.json \
    -s <MODEL AND METRICS DIR> \
    --include-package tre
```

## Evaluation

```bash
CUDA_VISIBLE_DEVICES=0 python ./experiments/utils/pr_curve_and_predictions.py \
    <MODEL AND METRICS DIR> \
    ./data/open_nre_nyt/test.json \
    --output-dir <RESULTS DIR> \
    --archive-filename <MODEL ARCHIVE FILENAME>
```

## Trained Models

The model(s) we trained on NYT to produce our paper results can be found here:

| Dataset  | Masking Mode    | AUC    | Download                                                                    |
| -------- | --------------- | ------ | --------------------------------------------------------------------------- |
| NYT      | None            | 0.422  | [Link](https://cloud.dfki.de/owncloud/index.php/s/jJit9giM325MfJA/download) |

### Download and extract model files

Download the archive corresponding to the model you want to evaluate (links in the table above).

```bash
wget --content-disposition <DOWNLOAD URL>
```

### Run evaluation

For example, to evaluate the NYT model used in the paper, run the following command:

```bash
CUDA_VISIBLE_DEVICES=0 python ./experiments/utils/pr_curve_and_predictions.py \
    <DIR CONTAINING THE MODEL ARCHIVE> \
    ./data/open_nre_nyt/test.json \
    --output-dir ./results/ \
    --archive-filename model_lm05_wu2_do2_bs16_att.tar.gz
```

## Citations
If you use our code in your research or find our repository useful, please consider citing our work.

```
@inproceedings{alt-etal-2019-fine,
    title = "Fine-tuning Pre-Trained Transformer Language Models to Distantly Supervised Relation Extraction",
    author = {Alt, Christoph  and
      H{\"u}bner, Marc  and
      Hennig, Leonhard},
    booktitle = "Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics",
    month = jul,
    year = "2019",
    address = "Florence, Italy",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/P19-1134",
    pages = "1388--1398",
}
```

## License
DISTRE is released under the Apache 2.0 license. See [LICENSE](LICENSE) for additional details.
