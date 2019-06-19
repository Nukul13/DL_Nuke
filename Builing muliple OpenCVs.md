# Build and Install OpenCV 3.4  and OpenCV 4.0 in a system running on Ubuntu

1. Open terminal by pressing `Ctrl + Alt + T`. Go to $HOME directory: `cd $HOME`

2. Remove previously installed opencv: 

```
sudo apt-get purge libopencv*
sudo apt autoremove
sudo apt-get update
sudo apt-get dist-upgrade
```

3. Update GCC: `sudo apt-get install --only-upgrade g++-5 cpp-5 gcc-5`

4. Install dependencies
```
sudo apt-get install build-essential make cmake cmake-curses-gui g++ libavformat-dev libavutil-dev libswscale-dev libv4l-dev libeigen3-dev libglew-dev libgtk2.0-dev libdc1394-22-dev libxine2-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libjpeg8-dev libjpeg-turbo8-dev libtiff5-dev libjasper-dev libpng12-dev libavcodec-dev libxvidcore-dev libx264-dev libgtk-3-dev libatlas-base-dev gfortran qt5-default python3-dev python3-pip python3-tk
sudo pip3 install numpy
sudo pip3 install matplotlib
```

5. Download and compile opencv-3.4.0 source code
```
wget https://github.com/opencv/opencv/archive/3.4.0.zip -O opencv-3.4.0.zip
unzip opencv-3.4.0.zip
cd opencv-3.4.0
wget https://github.com/opencv/opencv_contrib/archive/3.4.0.zip -O opencv_contrib-3.4.0.zip
unzip opencv-3.4.0.zip
mkdir build && cd build
mkdir installed
```
```
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=$HOME/opencv-3.4.0/build/installed -D WITH_CUDA=ON -D CUDA_ARCH_BIN="" -D CUDA_ARCH_PTX="" -D WITH_CUBLAS=ON -D ENABLE_FAST_MATH=ON -D CUDA_FAST_MATH=ON -D ENABLE_NEON=ON -D WITH_LIBV4L=ON -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=ON -D PYTHON_EXECUTABLE="usr/bin/python3" -D INSTALL_PYTHON_EXAMPLES=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D OPENCV_EXTRA_MODULES_PATH="../opencv_contrib-3.4.0/modules" ..
```
`make -j4`

`sudo make install`

6. Download and compile opencv-4.0.0 source code
```
wget https://github.com/opencv/opencv/archive/4.0.0.zip -O opencv-4.0.0.zip
unzip opencv-4.0.0.zip
cd opencv-4.0.0
wget https://github.com/opencv/opencv_contrib/archive/4.0.0.zip -O opencv_contrib-4.0.0.zip
unzip opencv-4.0.0.zip
mkdir build && cd build
mkdir installed
```
```
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=$HOME/opencv-4.0.0/build/installed -D WITH_CUDA=ON -D CUDA_ARCH_BIN="" -D CUDA_ARCH_PTX="" -D WITH_CUBLAS=ON -D ENABLE_FAST_MATH=ON -D CUDA_FAST_MATH=ON -D ENABLE_NEON=ON -D WITH_LIBV4L=ON -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=ON -D PYTHON_EXECUTABLE="usr/bin/python3" -D INSTALL_PYTHON_EXAMPLES=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D OPENCV_EXTRA_MODULES_PATH="../opencv_contrib-4.0.0/modules" ..
```

`make -j4`

`sudo make install`

7. Add opencv path in system. For that open .bashrc file
`gedit ~/.bashrc`

8. Now add the following code at the end of the file
```
if [[ $OPENCV == 3 ]]; then
   #OpenCV3 and other custom built libraries
   export LD_LIBRARY_PATH=/home/ubuntu/OpenCV/build/installed/lib:$LD_LIBRARY_PATH
   export OpenCV_DIR=/home/ubuntu/OpenCV/build/installed/:$OpenCV_DIR
   export PYTHONPATH=/home/ubuntu/OpenCV/build/installed/lib/python3.5/dist-packages/:$PYTHONPATH
elif [[ $OPENCV == 4 ]]; then
   #OpenCV4 and other custom built libraries
   export LD_LIBRARY_PATH=/home/ubuntu/opencv-4.0.0/build/installed/lib/:$LD_LIBRARY_PATH
   export OpenCV_DIR=/home/ubuntu/opencv-4.0.0/build/installed/:$OpenCV_DIR
   export PYTHONPATH=/home/ubuntu/opencv-4.0.0/build/installed/python/cv2/python-3.5/:$PYTHONPATH
fi

OPENCV=3
```

9. Now we configured this setup for OpenCV-3.4.0 and OpenCV-4.0.0. Now in order to switch between the different version, open the .bashrc file and set the version in “OPENCV” variable in the last line
`gedit ~/.bashrc`
Set “`OPENCV=3`” for using OpenCV 3.4.0
Set “`OPENCV=4`” for using OpenCV 4.0.0
`source ~/.bashrc`

10. Run Time Bindings of library
`sudo ldconfig`

11. To verify the installation
`python3 -c 'import cv2; print(cv2.__version__)'