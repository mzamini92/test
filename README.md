<div align="center">

<h1>DAP-CLIP: DINO-Affinity Proxy for Training-Free CLIP Segmentation</h1>

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
> *Training-free open-vocabulary semantic segmentation aims to transfer vision--language models to dense prediction without task-specific supervision. Despite recent progress, existing approaches often suffer from fragmented object regions, boundary leakage, and spurious background activations, particularly under large domain shifts and fine-grained object structures. 
We introduce \emph{Dense Aggregation via Proxies (DAP)}, a patch-affinity-guided attention mechanism that replaces the native self-attention weights in the final CLIP visual transformer block with an externally induced patch affinity matrix. Patch affinities are derived from external visual representations and optionally constrained to enforce region-level consistency, while CLIP’s value projections, text--image alignment, and training-free setting remain unchanged. By propagating semantic affinity through proxy-consistent neighborhoods, DAP promotes coherent object regions, sharper boundaries, and effective background suppression without relying on additional training or explicit mask supervision. Extensive experiments across eight benchmarks and multiple CLIP backbones demonstrate that DAP consistently improves over prior training-free methods, generalizes across diverse CLIP variants, and scales favorably with model capacity.*


## Dependencies and Installation


```
# create new anaconda env
conda create -n DAP python=3.10
conda activate DAP

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

In this repo, we integrate our DAP into ProxyCLIP (using sam, mae, dino, dinov2 as the VFMs). Please check `hyper_param.txt` for the hyper parameters.

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
```
python eval_all.py
```
Results will be saved in `results.xlsx`.

