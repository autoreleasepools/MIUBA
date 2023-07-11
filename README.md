# Exploring Adversarial Robustness of Vision-Language Pre-training Models: A Study on Modal Interaction and Defense Mechanisms
## Introduction
Considering its excellent performance on downstream tasks, vision-language pre-training model (VLP) have experienced explosive development. However, research on their adversarial robustness remains largely unexplored. We propose a multimodal attack method on the VLP model called \textit{Mutual Information Upper Bound Minimization Multimodal Attack} (MIUBA), and investigate the adversarial robustness of VLP models through it, particularly the modal interaction module. Additionally, to mitigate the impact of adversarial attacks on the VLP model, we design an adversarial sample detection method based on the differences of composite feature-gradient during modal interaction. Experimental results demonstrate that MIUBA achieves strong attack performance on different VLP models and downstream tasks, exposing the vulnerability of the modal interaction mechanism in VLP models. Our detection method exhibits competitive performance in detecting both single-modal and multimodal attacks, compared to other attacks, MIUBA has demonstrated a slightly higher level of evasion against our detection method. Analyzing the weaknesses of VLP models and developing novel detection methods hopefully contribute to their secure deployment in various applications. 
## Requirements
- Python 3.9+
- Pytorch 1.13.0
- For more detailed requirements, run
```
pip install -r requirements.txt
```
## Preparation
- Dataset json files for downstream tasks in [here](https://github.com/salesforce/ALBEF).
- Finetuned checkpoint for [ALBEF](https://github.com/salesforce/ALBEF).
- Finetuned checkpoint for [TCL](https://github.com/uta-smile/TCL).
- Finetuned checkpoint for VE task with [ALBEF and TCL](https://pan.baidu.com/s/1hHkSBgv23rx0zSywBXwwWA?pwd=iqvp).
- [MSCOCO train 2014 dataset](http://images.cocodataset.org/zips/train2014.zip)
- [MSCOCO val 2014 dataset](http://images.cocodataset.org/zips/val2014.zip)
- [Flickr30k dataset](https://www.kaggle.com/datasets/hsankesara/flickr-image-dataset)
- [SNLI-VE dataset](https://github.com/necla-ml/SNLI-VE)
- [RefCOCO+ dataset](https://github.com/lichengunc/refer)
## Evaluation
### MIUBA
#### Image-Text Retrieval
```
# Attack ALBEF/TCL
python RetrievalFusionEval.py --adv 5 --gpu 0 \
--config configs/Retrieval_flickr.yaml \
--output_dir output/Retrieval_flickr \
--checkpoint [Finetuned checkpoint]

# Attack Clip 
python RetrievalCLIPEval.py --adv 5 --gpu 0 --image_encoder ViT-B/16 \
--config configs/Retrieval_flickr.yaml \
--output_dir output/Retrieval_flickr \
--checkpoint [Finetuned checkpoint]
```
#### Visual Grounding
```
# Attack ALBEF
python GroundingFusionEval.py --adv 5 --gpu 0\
--config configs/Grounding.yaml \
--output_dir output/Grounding \
--checkpoint [Finetuned checkpoint]
```
#### Visual Entailment
```
# Attack ALBEF/TCL
python VEFusionEval.py --adv 5 --gpu 0 \
--config configs/VE.yaml \
--output_dir output/VE \
--checkpoint [Finetuned checkpoint]
```
### Multimodal Adversarial Sample Detection
#### Data Preparision
```
python VEDefendGetAllSample.py --adv 5 --gpu 0 \
--config configs/VE.yaml \
--output_dir output/VE \
--checkpoint [Finetuned checkpoint] \
--big_batch True
```
#### Train and Evaluate
```
python VEDefendMultiFeature.py --adv 5 --gpu 0 \
--config configs/VE.yaml \
--output_dir output/VE \
--checkpoint [Finetuned checkpoint] \
--big_batch True
```
#### Comparation 
```
python VEDefendMLLOO.py --adv 5 --gpu 0 \
--config configs/VE.yaml \
--output_dir output/VE \
--checkpoint [Finetuned checkpoint]
```
```
python VEDefendNLP.py --adv 5 \
--config configs/VE.yaml \
--output_dir output/VE
```
## Reference
- [Co-Attack](https://github.com/adversarial-for-goodness/Co-Attack)
