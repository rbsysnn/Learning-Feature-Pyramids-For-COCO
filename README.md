# Training COCO 2017 Object Detection and Segmentation via Learning Feature Pyramids

The code is provided by [Guangrun Wang](https://wanggrun.github.io/).

Sun Yat-sen University (SYSU)

### Table of Contents
0. [Introduction](#introduction)
0. [COCO](#coco)
0. [Citation](#citation)

### Introduction

This repository contains the training & testing code on [COCO 2017](http://cocodataset.org/#home) object detection and instance segmentation via learning feature pyramids (LFP). LFP is originally used for human pose machine, described in the paper "Learning Feature Pyramids for Human Pose Estimation" (https://arxiv.org/abs/1708.01101). We extend it to the object detection and instance segmentation.


### Results

These models are trained with different configurations on COCO 2017 training set and evaluated on COCO 2017 validation set.
MaskRCNN results contain both bbox and segm mAP. 

+ COCO Object Detection

|Method|`MASKRCNN_BATCH`|resolution |schedule| AP bbox | AP bbox 50 | AP bbox 75
|   -    |    -         |    -      |   -    |   -     |   -        |   -       |
|ResNet-50-C4  |512     |(800, 1333)|360k    |37.7     |   57.9     |   40.9    |
|Ours  |512             |(800, 1333)|360k    |39.8     |   60.2     |   43.4    |


+ COCO Instance Segmentation


|Method|`MASKRCNN_BATCH`|resolution |schedule| AP mask | AP mask 50 | AP mask 75
|   -    |    -         |    -      |   -    |   -     |   -        |   -       |
|ResNet-50-C4  |512     |(800, 1333)|360k    |32.8     |   54.3     |   34.7    |
|Ours  |512             |(800, 1333)|360k    |34.6     |   56.7     |   36.8    |

The two R50-C4 360k models have the same configuration __and mAP__
as the `R50-C4-2x` entries in
[Detectron Model Zoo](https://github.com/facebookresearch/Detectron/blob/master/MODEL_ZOO.md#end-to-end-faster--mask-r-cnn-baselines).
The other models listed here do not correspond to any configurations in Detectron.



### ImageNet

+ First:
```
cd pyramid/ImageNet/ 
```

+ Training script:
```
python imagenet-resnet.py   --gpu 0,1,2,3,4,5,6,7   --data_format NHWC  -d 101  --mode resnet --data  [ROOT-OF-IMAGENET-DATASET]
```

+ Testing script:
```
python imagenet-resnet.py   --gpu 0,1,2,3,4,5,6,7  --load [ROOT-TO-LOAD-MODEL]  --data_format NHWC  -d 101  --mode resnet --data  [ROOT-OF-IMAGENET-DATASET] --eval
```

+ Trained Models:

   [Baidu Pan](https://pan.baidu.com/s/1SKEmrjcYA-NR9oFBOD7Y2w), code: 269o

   [Google Drive](https://drive.google.com/drive/folders/1pVSCQ6gap0b73FFr8bF-5p5Am6e2rXRr?usp=sharing)

### PASCAL VOC2012

+ First:
```
cd pyramid/VOC/
```

+ Training script:
```
# Use the ImageNet classification model as pretrained model.
# Because ImageNet has 1,000 categories while voc only has 21 categories, 
# we must first fix all the parameters except the last layer including 21 channels. We only train the last layer for adaption
# by adding: "with freeze_variables(stop_gradient=True, skip_collection=True): " in Line 206 of resnet_model_voc_aspp.py
# Then we finetune all the parameters.
# For evaluation on voc val set, the model is first trained on COCO, then on train_aug of voc. 
# For evaluation on voc leaderboard (test set), the above model is further trained on voc val.
# it achieves 81.0% on voc leaderboard.
# a training script example is as follows.
python resnet-msc-voc-aspp.py   --gpu 0,1,2,3,4,5,6,7  --load [ROOT-TO-LOAD-MODEL]  --data_format NHWC  -d 101  --mode resnet --log_dir [ROOT-TO-SAVE-MODEL]  --data [ROOT-OF-TRAINING-DATA]
```

+ Testing script:
```
python gr_test_pad_crf_msc_flip.py 
```


+ Trained Models:

   Model trained for evaluation on voc val set:

   [Baidu Pan](https://pan.baidu.com/s/1C7r10EeZEOIn0njRCuuR1g), code: 7dl0

   [Google Drive](https://drive.google.com/drive/folders/1c3Fr6yC_rwXGF4hJB4ADmtd2AF8wsO51?usp=sharing)

   Model trained for evaluation on voc leaderboard (test set)

   [Baidu Pan](https://pan.baidu.com/s/1C7r10EeZEOIn0njRCuuR1g), code: 7dl0

   [Google Drive](https://drive.google.com/drive/folders/1c3Fr6yC_rwXGF4hJB4ADmtd2AF8wsO51?usp=sharing)

### Citation

If you use these models in your research, please cite:

	@inproceedings{yang2017learning,
            title={Learning feature pyramids for human pose estimation},
            author={Yang, Wei and Li, Shuang and Ouyang, Wanli and Li, Hongsheng and Wang, Xiaogang},
            booktitle={The IEEE International Conference on Computer Vision (ICCV)},
            volume={2},
            year={2017}
        }

### Dependencies
+ Python 2.7 or 3
+ TensorFlow >= 1.3.0
+ [Tensorpack](https://github.com/ppwwyyxx/tensorpack)
   The code depends on Yuxin Wu's Tensorpack. For convenience, we provide a stable version 'tensorpack-installed' in this repository. 
   ```
   # install tensorpack locally:
   cd tensorpack-installed
   python setup.py install --user
   ```

