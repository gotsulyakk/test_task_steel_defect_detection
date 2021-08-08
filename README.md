# Steel defect detection [test task]

## Install
```sh
git clone https://github.com/gotsulyakk/test_task_steel_defect_detection.git
cd test_task_steel_defect_detection

pip install -r requirements.txt
```
Aim is to create simple binary segementation pipeline using [pytorch lightning](https://www.pytorchlightning.ai/), [segmentation_models.pytorch](https://github.com/qubvel/segmentation_models.pytorch) and data from [severstal steel defect detection challenge](https://www.kaggle.com/c/severstal-steel-defect-detection/overview)
## Data
[Original dataset](https://www.kaggle.com/c/severstal-steel-defect-detection/data) contains both labeled and unlabeled grayscale (ith 3 channels) images with defects (4 classes) and without defects. Goal of this task is to combine 4 classes into 1 and do binary segmentation. 

Data prepared with notebooks/data_prep.ipynb

- Original image resolution: 256x1600
- Image resolution during training: 256x416
- Number of images used in this task: 800 for train, 100 for validation, 100 for test set.

## Result
Model have **~50 IoU** on test set

## Training
You can run training command with:
```sh
python train.py --config {PATH/TO/CONFIG}
```
**Architecture**: I tried Unet and FPN architectures. FPN seems to work slight better (~1%).
**Encoder**: resnext50_32x4d was ~2% better than efficientnetb3 in my few runs.
**Loss function**: Dice
**Metric**: IoU(threshold=0.5)
**Number of epochs**: 30

## Inference example
You can run demo.py for inference:
```sh
python demo.py --image {PATH/TO/IMAGE} --model_ckpt {PATH/TO/MODEL_CKPT} --config {PATH/TO/CONFIG}
```
Random image from test set:
![Original image](https://github.com/gotsulyakk/test_task_steel_defect_detection/blob/main/data/inference/original.jpg)
![Predicted mask](https://github.com/gotsulyakk/test_task_steel_defect_detection/blob/main/data/inference/original.jpg)
![Image with mask](https://github.com/gotsulyakk/test_task_steel_defect_detection/blob/main/data/inference/original.jpg)
![Ground truth mask](https://github.com/gotsulyakk/test_task_steel_defect_detection/blob/main/data/inference/original.jpg)

## Further improvements
There are various technics to improve the model:
1. The most common - add more data
2. Try another architecture and/or another backbone
3. Try another loss function or combine them
4. Try another optimizer
5. Play with image resolution which require more computational power
5. Find best learning rate
4. If we don't want to "label" more data we can "pseudolabel" it and use for training 

## Problems
