
# Automatic Number Plate Recognition

Automatic Number Plate Recognition (ANPR) is a two step process. First the number plate is detected. After detection, that specific region is cropped which is then passed through the OCR which extract all the characters. We can use that extracted data for different purpose like storing it in a database or sending it to dashboard etc. 

## Download the dataset

Make a separate folder in your drive. You should upload your dataset and save all the project files here.
You can Download the dataset from [here](https://linktodocumentation) or you can manually label your dataset using [roboflow](https://roboflow.com/). Make your free account on roboflow, upload your data and start annotation. Save the annotations in yolov5 format. Export the data directly to your Drive by using the code snippet provided by the roboflow or you can Download the dataset and upload it to your drive. Directly exporting the dataset to the drive is little easy and it save some time so I would recomment that. 

## Steps to follow on Google Colab
These are the steps to be followed on Google Colab for  training the Automatic Number Plate Recogntion. 


First mount the drive 
``` bash
from google.colab import drive
drive.mount('/content/drive')
```

Change the working directory to the folder created recently which is having the dataset

``` bash
  %cd path/to/your/folder/here

```
Check the present working directory by
``` bash
!pwd
```


Clone the yolov5 repo

```bash
  !git clone https://github.com/ultralytics/yolov5 
```

Go to the project directory

```bash
  %cd yolov5
```

Install dependencies

```bash
  !pip install -r requirements.txt
```



## Setting for Custom training

Create a yaml file by the name of ```number_plates.yaml``` inside the ```data``` directory which is inside the ```yolov5``` folder.

Paste the following data in it. Provide the path as per your case for the ```train``` and ```val```. 

``` yaml
train: "path to your training folder"
val: "path to your validation folder"

# number of classes -> here it is 1 (number plates)
nc: 1

# class names
names: [ 'number_plate']

```
Save the ```number_plates.yaml``` file and then start the training. 


## Training
Make sure you have selected the GPU for training on Google Colab otherwise it would take a lot of time to do the training. 

``` bash
!python train.py --img 640 --batch 16 --epochs 5 --data number_plates.yaml --weights yolov5x.pt

```

This will start the training training for 5 epochs. You can change the epochs for better results. 


## Inference on test data

Provide the correct path to saved weights and the test images. 

``` bash
!python detect.py --weights runs/train/exp/weights/best.pt --img 416 --conf 0.1 --source vehicles-number-plates-detection-1/test/images/
```

Here we test the model for object detection on the provided test data. Automatic number plates recognition is a two step process. First the number plates is detected, then cropped and after that, the cropped portion is passed through the OCR which extract the data out of it. We can then save the data in our database. 
All the results are saved in ```runs/detect```


An example notebook is provided in this repo which is giving the detailed explanation of the whole process along with the code ranging from object detect ( number plate detection ) to the OCR process and showing the results on the image. Please check it out.