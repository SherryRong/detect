**download and install anaconda.** https://www.anaconda.com/products/individual#Downloads

**then create yolo environment in conda ：**

In [ ]:

```
conda create -n yolo python=3.8 
conda info -e
activate yolo # switch environment from base to yolo
```

**download yolov5.** https://github.com/ultralytics/yolov5

**install requirements under yolo environment：**

In [ ]:

```
pip install -r requirements.txt
```

**create a folder 'yolo', including a folder 'images', a folder 'lables' and a yaml file 'A.yaml'.**

**collecte several images of the object you want to detect and put them into 'images' folder.**

**download labelimg. labelimg is a tool to help you lable collected images.** https://github.com/tzutalin/labelImg

(**you can also use Roboflow to lable.** https://roboflow.com/?ref=ultralytics)

**after unzip labelimg folder, delete 'labelImg-master\data\predefined_classes.txt'. Otherwise there will be some strange classifications.**

In [ ]:

```
activate yolo
conda install pyqt=5
conda install -c anaconda lxml
pyrcc5 -o libs/resources.py resources.qrc
python labelimg.py
```

**use labelimg select the object in the images and save data in labels folder.**

**put following code in the A.yaml file and change file path.**

In [ ]:

```
# train and val data as 1) directory: path/images/, 2) file: path/images.txt, or 3) list: [path1/images/, path2/images/]
train: ../yolo/images/
val: ../yolo/images/
# number of classes
nc: 1

# class names
names: ['A']
```

**train your dataset.**

In [ ]:

```
import torch
from IPython.display import Image, clear_output  # to display images

clear_output()
print('Setup complete. Using torch %s %s' % (torch.__version__, torch.cuda.get_device_properties(0) if torch.cuda.is_available() else 'CPU'))

python train.py --img 640 --batch 50 --epochs 100 --data ../yolo/A.yaml --weights yolov5s.pt --nosave --cache
```

**after training, use another image to test. change image path.**

In [ ]:

```
python detect.py --weights /content/yolov5/runs/train/exp2/weights/best.pt --img 640 --conf 0.25 --source ../test.jpg
```

**show detect result.**

In [ ]:

```
import matplotlib.pyplot as plt
from matplotlib.image import imread
 
img = imread('runs\detect\exp7\\test.jpg')
plt.imshow(img)
 
plt.show()
```