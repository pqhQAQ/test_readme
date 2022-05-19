## 一、下载yolov5

```text
git clone https://github.com/ultralytics/yolov5.git
git checkout v3.0
wget https://github.com/ultralytics/yolov5/releases/download/v3.0/yolov5s.pt
```



## 二、修改代码



1、AttributeError: 'Hardswish' object has no attribute 'inplace'

参考：[__https://blog.csdn.net/ynxdb2002/article/details/109697772__](https://blog.csdn.net/ynxdb2002/article/details/109697772)

```text
vi /home/panqiuhong/.local/lib/python3.8/site-packages/torch/nn/modules/activation.py
modify line:461
```

2、AttributeError: 'Detect' object has no attribute 'export'

```text
vi models/yolo.py
add line：19 export = False  # onnx export
```

3、a view of a leaf Variable that requires grad is being used in an in-place operation

参考：[__https://github.com/ultralytics/yolov5/issues/1552__](https://github.com/ultralytics/yolov5/issues/1552)

```text
vi /home/panqiuhong/satellite/yolov5/models/yolo.py
modify line:145
```

![](https://github.com/pqhQAQ/images/blob/main/yolov5s_train_1.png "")

4、TypeError: can't convert cuda:0 device type tensor to numpy. Use Tensor.cpu() to copy the tensor to host memory first.

```text
vi /home/panqiuhong/.local/lib/python3.8/site-packages/torch/_tensor.py
modify line:678 self.cpu().numpy()
```



## 三、训练

1、运行推理验证环境

python3 detect.py --source inference/images/bus.jpg --weights yolov5s.pt

2、添加satellite.yaml

![](https://github.com/pqhQAQ/images/raw/main/yolov5s_train_satellite.png "")

3、修改yolov5s.yaml

![](https://github.com/pqhQAQ/images/raw/main/yolov5s_train_yolov5s.png "")

4、执行训练

python3 train.py --weights yolov5s.pt --data data/satellite.yaml --batch 256 --epoch 500

