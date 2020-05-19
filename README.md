# Tutorial to set up TensorFlow Object Detection API on the Raspberry Pi
The Python script in this repository, main.py, detects objects in live feeds from a Picamera or USB webcam. The script loads the model into memory, initializes the Picamera, and then begins performing object detection on each video frame from the Picamera. 


Run the script by issuing: 
```
python3 main.py 
```
The script defaults to using an attached Picamera. If you have a USB webcam instead, add --usbcam to the end of the command:
```
python3 main.py --usbcam
```

Once the script initializes (which can take up to 30 seconds), you will see a window showing a live view from your camera. Common objects inside the view will be identified and have a rectangle drawn around them. 


## Installation
### 1. Update the Raspberry Pi
```
sudo apt-get update
sudo apt-get dist-upgrade
```
### 2. Install Tensorflow

__Install TensorFlow:__
```
pip3 install tensorflow
```

__Install dependencies for using TensorFlow Object Detection API:__
```
sudo apt-get install libatlas-base-dev
sudo pip3 install pillow lxml jupyter matplotlib cython
sudo apt-get install python-tk
```


### 3. Install OpenCV
OpenCV is used to visualize images (instead of matplotlib like TensorFlow)
```
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install qt4-dev-tools libatlas-base-dev
```
```
sudo pip3 install opencv-python
```

### 4. Install Simpleaudio
This is used for the audio output.
```
pip3 install simpleaudio
```



### 4. Compile and Install Protobuf
Protobuf is a package that implements Google’s Protocol Buffer data format.
```
sudo apt-get install protobuf-compiler
```

Run `protoc --version` once that's done to verify it is installed. You should get a response of `libprotoc 3.6.1` or similar.


### 5. Set up TensorFlow Directory Structure and PYTHONPATH Variable
Now we will set up the TensorFlow directory. Go to your home directory, then make a directory called “tensorflow1”, and cd into it.
```
mkdir tensorflow1
cd tensorflow1
```
Download the tensorflow repository from GitHub by issuing:
```
git clone --depth 1 https://github.com/tensorflow/models.git
```
Modify the PYTHONPATH environment variable to point at some directories inside the TensorFlow repository we just downloaded. PYTHONPATH is set every time we open a terminal, so we have to modify the .bashrc file. Open it by issuing:
```
sudo nano ~/.bashrc
```
Move to the end of the file, and on the last line, add:
```
export PYTHONPATH=$PYTHONPATH:/home/pi/tensorflow1/models/research:/home/pi/tensorflow1/models/research/slim
```

Save and exit the file. This makes it so the “export PYTHONPATH” command is called every time you open a new terminal, so the PYTHONPATH variable will always be set appropriately. Close and then re-open the terminal.

Now we use Protoc to compile the Protocol Buffer (.proto) files used by the TensorFlow Object Detection API. The .proto files are located in /research/object_detection/protos, but we need to execute the command from the /research directory. Issue:
```
cd /home/pi/tensorflow1/models/research
protoc object_detection/protos/*.proto --python_out=.
```

This command converts all the "name".proto files to "name_pb2".py files. Next, move into the object_detection directory:
```
cd /home/pi/tensorflow1/models/research/object_detection
```

Download the SSD_Lite model from the [TensorFlow detection model zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md). The model zoo is Google’s collection of pre-trained object detection models that have various levels of speed and accuracy. The Raspberry Pi has a weak processor, so we need to use a model that takes less processing power. Though the model will run faster
It is a tradeoff of having lower accuracy. We will use SSDLite-MobileNet, which is the fastest model available. 

Google is continuously releasing models with improved speed and performance, so check back at the model zoo often to see if there are any better models.

Download the SSDLite-MobileNet model and unpack it by issuing:
```
wget http://download.tensorflow.org/models/object_detection/ssdlite_mobilenet_v2_coco_2018_05_09.tar.gz
tar -xzvf ssdlite_mobilenet_v2_coco_2018_05_09.tar.gz
```

### 7. Enable Camera

If you’re using a Picamera, make sure it is enabled in the Raspberry Pi configuration menu.


## Runnning
Run the script by issuing: 
```
python3 Object_detection_picamera.py 
```
The script defaults to using an attached Picamera. If you have a USB webcam instead, add --usbcam to the end of the command:
```
python3 Object_detection_picamera.py --usbcam
```

Once the script initializes (which can take up to 30 seconds), you will see a window showing a live view from your camera. Common objects inside the view will be identified and have a rectangle drawn around them. 


## References
* See [EdjeElectronics](https://github.com/EdjeElectronics/TensorFlow-Object-Detection-on-the-Raspberry-Pi) GitHub Project as similar project of object detection using Raspberry Pi.
* TensorFlow Object Detection](https://www.tensorflow.org/lite/models/object_detection/overview)
* [OpenCV](https://opencv.org)
* [PyGame](https://www.pygame.org/news)


