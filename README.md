# Lab2Pix

![](./demo/demo.png)

Lab2Pix: Label-Adaptive Generative Adversarial Network for Unsupervised Image Synthesis

Code will be released soon

## Installztion
Clone this repo.

```bash
git clone https://github.com/Wowil/Lab2Pix.git
cd Lab2Pix/
```

This code requires PyTorch 0.4.0+ and python 3+. PyTorch 1.0.0 + and python 3.6.0 + are recommonded. Please install dependencies by

```bash
pip install -r requirements.txt
```

## Dataset
All the datasets used in this research must be downloaded beforehand.

**Cityscapes Dataset** can be downloaded [here](https://www.cityscapes-dataset.com/). In particular, you will need to download leftImg8bit_trainvaltest.zip, gtFine_trainvaltest.zip. The images and labels should be arranged as below:

```
Lab2Pix
|───datasets
|   |───cityscapes
|   |   |───leftImg8bit
|   |   |   |───train
|   |   |   |   |───aachen
|   |   |   |   |   |───aachen_000000_000019_leftImg8bit.png
|   |   |   |───val
|   |   |   |   |───frankfurt
|   |   |   |   |   |───frankfurt_000000_000294_leftImg8bit.png
|   |   |───gtFine
|   |   |   |───train
|   |   |   |   |───aachen
|   |   |   |   |   |───aachen_000000_000019_gtFine_color.png
|   |   |   |   |   |───aachen_000000_000019_gtFine_labelIds.png
|   |   |   |───val
|   |   |   |   |───frankfurt
|   |   |   |   |   |───frankfurt_000000_000294_gtFine_color.png
|   |   |   |   |   |───frankfurt_000000_000294_gtFine_labelIds.png
```

**Facades Dataset** can be downloaded [here](http://cmp.felk.cvut.cz/~tylecr1/facade/). In particular, you will need to preprocess the original dataset by splitting the aligned image into unaligned image pair. The crop function provided by PIL.Image is recommonded.  The images and labels should be arranged as below:

```
Lab2Pix
|───datasets
|   |───facades
|   |   |───Image
|   |   |   |───train
|   |   |   |   |───1.jpg
|   |   |   |───val
|   |   |   |   |───1.jpg
|   |   |───Label
|   |   |   |───train
|   |   |   |   |───1_color.jpg
|   |   |   |───val
|   |   |   |   |───1_color.jpg
```

**edges2shoes Dataset** can be downloaded following the [website](https://github.com/phillipi/pix2pix). The images and labels should be arranged as below:

```
Lab2Pix
|───datasets
|   |───facades
|   |   |───train
|   |   |   |───1_AB.jpg
|   |   |───val
|   |   |   |───1_AB.jpg
```
**edges2handbags Dataset** can be downloaded following the [website](https://github.com/phillipi/pix2pix). The file structure should be arranged as the same as edges2shoes Dataset.

## Train New Models
Before you train new models, make sure you have prepared the datasets in proper structure. If you place dataset in other direction, you can use ```--dataroot [path to dataset]``` to specify the correct direction. Note that, the dataset root filename like 'cityscapes' can not be renamed. You can also change the save path of the checkpoints by specifying ```--checkpoints_dir [path to checkpoints]```. If you have a pretrained model or your program crash during the training, you can continute training by adding ```--continue_train``` and specify the current model path ```--load_pretrain [path to checkpoint]```.

Then you can train your model
```bash
# To train on cityscapes or edges2shoes dataset, for example
python train.py --name [experiment name] --dataroot cityscapes --batchSize 1
python train.py --name [experiment name] --dataroot edges2shoes --batchSize 4
```

If you have GPUs more than one or over 12GB memory, you can use ```--gpu_ids [GPU ids split with ","]``` to specify multi GPUs working (e.g. ```--gpu_ids 0,1,2,3```) or you can increase the batch size ```--batchSize [batch size number]```.

## Test Models
If you have a pretrained model, you can test it with the following commond
```bash
python test.py --name [experiment name] --dataroot [dataset dir] --results_dir [test_reults save path]
or
python test.py --load_pretrain [path to checkpoint] --dataroot [dataset dir] --results_dir [test_reults save path]
```

If you don't want to test all samples in datasets, you can specify ```--max_test_num [the max number of the test]```. By default, the progress loads the latest checkpoint. It can be changed using ```--which_epoch [epoch number]```.

