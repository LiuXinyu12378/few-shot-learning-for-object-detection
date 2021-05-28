# Few-shot YOLOv3: Few-shot Object Detection YOLOv3

#### 网络修改by xinyuuliu       交流请联系作者qq522414928

##### 环境 Python3.6  pytorch1.5+  easydict


## Model base on it
<div align=center>
<img src="https://user-images.githubusercontent.com/47288017/85242838-ab36fb00-b472-11ea-9ec8-418d06d057a0.png" width="740">
</div>


## 由Few-shot Object Detection via Feature Reweighting 改进

![](https://user-images.githubusercontent.com/8370623/67256408-ad583e00-f43b-11e9-806e-47d79acecaed.png)



使用数据VOC 20类    15个base类和5个few-shot类



### Base Training
Modify Config

Change the cfg/fewyolov3_voc.data file 

```
metayolo=1
metain_type=2
data=voc
neg = 1
rand = 0
novel = data/voc_novels.txt
novelid = 0
steps=-1,64000
scales=1,0.1
learning_rate = 0.001
meta = data/voc_traindict_full.txt
train = /data/liuxy67/Few-shot-Object-Detection/scripts/train.txt
valid = /data/liuxy67/Few-shot-Object-Detection/scripts/2007_test.txt
backup = backup/metayolov3_voc
gpus=0
num_workers=4
```

Train the Model
```
python train.py cfg/fewyolov3_voc.data cfg/darknet_yolov3_spp.cfg cfg/reweighting_net.cfg
```

Evaluate the Model
```
python valid.py cfg/fewyolov3_voc.data cfg/darknet_yolov3_spp.cfg cfg/reweighting_net.cfg path/toweightfile
python scripts/voc_eval.py results/path/to/comp4_det_test_ cfg/metayolo.data
```

### Few-shot Tuning
Modify Config for NWPU VHR-10 Data   
Change the cfg/fewtunev3_nwpu_10shot.data file (change the shot number to try different shots)
```
metayolo=1
metain_type=2
data=voc
tuning = 1
neg = 0
rand = 0
novel = data/voc_novels.txt
novelid = 0
max_epoch = 2000
repeat = 200
dynamic = 0
scale=1
train = ./scripts/train.txt
meta = data/voc_traindict_bbox_10shot.txt
valid = ./scripts/2007_test.txt
backup = backup/metatune
gpus  = 0
```


Train the Model with 10 shot
```
python train.py cfg/few_shot_data.data cfg/darknet_yolov3_spp.cfg cfg/reweighting_net.cfg path/to/base/weightfile
```

Evaluate the Model
```
python valid.py cfg/few_shot_data.data cfg/darknet_yolov3_spp.cfg cfg/reweighting_net.cfg path/to/tuned/weightfile
python scripts/voc_eval.py results/path/to/comp4_det_test_ cfg/fewtunev3_nwpu_10shot.data
```

## Acknowledgements
Large part of the code is borrowed from [YOLO-Low-Shot](https://github.com/bingykang/Fewshot_Detection)
