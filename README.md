# Step by step procedure to train a YOLOV3 model for apple detection and localization.

This repository is inspired by AlexeyAB'soriginal code.

## 1) Clone, configure & compile Darknet

```
git clone https://github.com/AlexeyAB/darknet
```

```
cd darknet
sed -i 's/OPENCV=0/OPENCV=1/' Makefile
sed -i 's/GPU=0/GPU=1/' Makefile
sed -i 's/CUDNN=0/CUDNN=1/' Makefile
```

```
make
```

## 2) Configure yolov3.cfg file

```
cp cfg/yolov3.cfg cfg/yolov3_training.cfg
```

```
sed -i 's/batch=1/batch=64/' cfg/yolov3_training.cfg
sed -i 's/subdivisions=1/subdivisions=16/' cfg/yolov3_training.cfg
sed -i 's/max_batches = 500200/max_batches = 2000/' cfg/yolov3_training.cfg
sed -i '610 s@classes=80@classes=1@' cfg/yolov3_training.cfg
sed -i '696 s@classes=80@classes=1@' cfg/yolov3_training.cfg
sed -i '783 s@classes=80@classes=1@' cfg/yolov3_training.cfg
sed -i '603 s@filters=255@filters=18@' cfg/yolov3_training.cfg
sed -i '689 s@filters=255@filters=18@' cfg/yolov3_training.cfg
sed -i '776 s@filters=255@filters=18@' cfg/yolov3_training.cfg
```

## 3) Create .names and .data files

```
echo -e 'Apple' > data/obj.names
echo -e 'classes= 1\ntrain  = data/train.txt\nvalid  = data/test.txt\nnames = data/obj.names\nbackup = provide the path to your folder where you want to save the trained weights' > data/obj.data
```

## 4) Save yolov3_training.cfg and obj.names files in the same folder where the trained weights are saved

```
cp cfg/yolov3_training.cfg provide the same path to the folder where you have saved the trained weights/yolov3_testing.cfg
cp data/obj.names provide the same path to the folder where you have saved the trained weights/classes.txt
```

## 5) Create a folder and unzip image dataset

```
mkdir data/obj
unzip provide the path to your dataset in zip format/images.zip -d data/obj
```

## 6) Create train.txt file

```
python
>>>import glob
>>>images_list = glob.glob("data/obj/*.png")
>>>with open("data/train.txt", "w") as f:
>>>    f.write("\n".join(images_list))
```

## 7) Download pre-trained weights for the convolutional layers file

```
wget https://pjreddie.com/media/files/darknet53.conv.74
```

## 8) Start training

```
./darknet detector train data/obj.data cfg/yolov3_training.cfg darknet53.conv.74 -dont_show
# Use below command to re-start your training from last saved weights
./darknet detector train data/obj.data cfg/yolov3_training.cfg provide the path to the folder where your weights are stored/yolov3_training_last.weights -dont_show
```






