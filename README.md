# ConMamba [![arXiv](https://img.shields.io/badge/arXiv-2407.09732-<COLOR>.svg)](https://arxiv.org/abs/2407.09732)

An implementation of convolution-augmented Mamba for speech recognition.

## Notebook Implementation
Google Colab Link : https://colab.research.google.com/drive/1DoHke3cXsdBnjverODNRAdQ0N9zoTMeM?usp=sharing

## Architecture

<img src="figures/conmamba.png" alt="conmamba" width="80%">
<img src="figures/mamba_encoder_decoder.png" alt="layers" width="80%">

## Prerequisites

1. Download LibriSpeech [corpus](https://www.openslr.org/12).

2. Install Packages.
```
conda create --name Slytherin python=3.9 --solver classic
conda init (Close and reopen shell after)
conda activate Slytherin
pip install -r requirements.txt
```
You may need to install lower or higher versions of torch, torchaudio, causal-conv1d and mamba-ssm based on your hardware and system. Make sure they are compatible. 


## Training
To train a ConMamba Encoder-Transformer Decoder model on one GPU:
```
python train_S2S.py hparams/S2S/conmamba_large(small).yaml --data_folder <YOUR_PATH_TO_LIBRISPEECH> --precision bf16 
```
To train a ConMamba Encoder-Mamba Decoder model on one GPU:
```
python train_S2S.py hparams/S2S/conmambamamba_large(small).yaml --data_folder <YOUR_PATH_TO_LIBRISPEECH> --precision bf16 
```
Note : On first run, ensure that the hparams.yaml file has the `skip_prep` parameter set to `False`. Then take the newly created train.csv files and put them in a new directory `/manifests`.
```
skip_prep: False
train_csv: !ref <output_folder>/train.csv
valid_csv: !ref <output_folder>/dev-clean.csv
test_csv:
    - !ref <output_folder>/test-clean.csv
    - !ref <output_folder>/test-other.csv
```

To train a ConMamba Encoder model with a character-level CTC loss on four GPUs:
```
torchrun --nproc-per-node 4 train_CTC.py hparams/CTC/conmamba_large.yaml --data_folder <YOUR_PATH_TO_LIBRISPEECH> --precision bf16 
```

## Inference and Checkpoints (Later)

## Performance (Word Error Rate%)
<img src="figures/performance.png" alt="performance" width="60%">

## Acknowledgement

Acknowledgement goes to the wonderful work of [Mamba](https://arxiv.org/abs/2312.00752) and [Vision Mamba](https://arxiv.org/abs/2401.09417). I borrowed their implementation of [Mamba](https://github.com/state-spaces/mamba) and [bidirectional Mamba](https://github.com/hustvl/Vim). The training recipes are adapted from [SpeechBrain](https://speechbrain.github.io).

<!-- ## Citation
If you find this work helpful, please consider citing: -->

<!-- ```bibtex
@misc{jiang2024speechslytherin,
      title={Speech Slytherin: Examining the Performance and Efficiency of Mamba for Speech Separation, Recognition, and Synthesis}, 
      author={Xilin Jiang and Yinghao Aaron Li and Adrian Nicolas Florea and Cong Han and Nima Mesgarani},
      year={2024},
      eprint={2407.09732},
      archivePrefix={arXiv},
      primaryClass={eess.AS},
      url={https://arxiv.org/abs/2407.09732}, 
}
``` -->

<!-- You may also like our Mamba for speech separation: https://github.com/xi-j/Mamba-TasNet -->

