<div align="center">

<h1>DouC: Dual-Branch CLIP for Training-Free Open-Vocabulary Segmentation</h1>

<div>
    <strong>ICML 2026 Submission</strong>
</div>


<div>
    <h4 align="center">
        • <a href="#" target='_blank'>[arXiv]</a> •
    </h4>
</div>

<br>

</div>

## TL;DR
> *Open-vocabulary semantic segmentation requires assigning pixel-level semantic labels while supporting an open and unrestricted set of categories. Training-free CLIP-based approaches preserve strong zero-shot generalization but typically rely on a single inference mechanism, limiting their ability to jointly address unreliable local tokens and insufficient spatial coherence. We propose \textbf{DouC}, a training-free dual-branch CLIP framework that decomposes dense prediction into two complementary components. OG-CLIP improves patch-level reliability via lightweight, inference-time token gating, while FADE-CLIP injects external structural priors through proxy attention guided by frozen vision foundation models. The two branches are fused at the logit level, enabling local token reliability and structure-aware patch interactions to jointly influence final predictions, with optional instance-aware correction applied as post-processing. DouC introduces no additional learnable parameters, requires no retraining, and preserves CLIP’s zero-shot generalization. Extensive experiments across eight benchmarks and multiple CLIP backbones demonstrate that DouC consistently outperforms prior training-free methods and scales favorably with model capacity.*


## Dependencies and Installation


```
# create new anaconda env
conda create -n DouC python=3.10
conda activate DouC

# install torch and dependencies
pip install -r requirements.txt
```


## Datasets
Please follow the [MMSeg data preparation document](https://github.com/open-mmlab/mmsegmentation/blob/main/docs/en/user_guides/2_dataset_prepare.md) and [ProxyCLIP](https://github.com/mc-lan/ProxyCLIP/tree/main) to download and pre-process the datasets. 
The COCO-Object dataset can be converted from COCO-Stuff164k by executing the following command:

```
python datasets/cvt_coco_object.py PATH_TO_COCO_STUFF164K -o PATH_TO_COCO164K
```

## Model evaluation

In this repo, we integrate our DouC into ProxyCLIP (using sam, mae, dino, dinov2 as the VFMs). Please check `hyper_param.txt` for the hyper parameters.

Please modify some settings in `configs/base_config.py` before running the evaluation.

The default VLM used in this code is [dino_vitb8](https://github.com/facebookresearch/dino). The same as the default one used in ProxyCLIP.

For SAM and MAE, please download the checkpoints from [SAM](https://github.com/facebookresearch/segment-anything#model-checkpoints) and [MAE](https://github.com/facebookresearch/mae).



Single-GPU:

```
python eval.py --config ./configs/cfg_DATASET.py --workdir YOUR_WORK_DIR
```

Multi-GPU:
```
bash ./dist_test.sh ./config/cfg_DATASET.py
```

Evaluation on all datasets:

for running Vit-B/16:
```
python eval_all.py
```
for running Vit-L/14 and H/14:
```
python eval_all1.py
```
Results will be saved in `results.xlsx`.

