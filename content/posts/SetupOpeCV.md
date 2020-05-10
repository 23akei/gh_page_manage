---
title: "Ubuntu上でOpenCV開発環境を構築"
date: 2020-05-11T00:56:53+09:00
draft: false
categories: ["技術","備忘録"]
tags: ["Ubuntu","OpenCV","環境構築"]
---

　Ubuntu上でOpenCVの環境構築をする際に結構手間がかかったのでその手順をまとめます。いままでオフラインのドキュメントで共有してましたが（主につくばロボットサークル内での技術継承のために、）ここに加筆修正してまとめます。
<br>
<br>
<br>

#### Version
　OpenCV 3.3.0 (他バージョンでもイケるかも)  
<br>
<br>
<br>

#### インストール環境  
　僕がこの手順で構築を行った環境です。(x86マシン上のUbuntu16.04ならだいたい大丈夫な気がする)  
　いずれもOSインストール直後の素の状態の環境で実行してます。  
+ Ubuntu16.04 on VirtualBox / Core i5-8265U / RAM 8GB
+ Ubuntu16.04 / Core i5-4300M / RAM 8GB  
<br>
<br>
<br>

#### OpenCVインストールの前に  
　デフォルトでは追加されてないレポジトリのパッケージを利用するため、追加しておく。  
```bash
$ sudo add-apt-repository ppa:jonathonf/ffmpeg-3
$ sudo apt update
```
<br>
<br>
<br>

#### 関連パッケージのインストール  
　なんかめっちゃいっぱいインストールする。 ~~ただしOpenCV本体ではない~~  
```bash
$ sudo apt install ffmpeg libav-tools x264 x265 lib4l-dev cmake libeigen3-dev libgtk-3-dev qt5-default freeglut3-dev libvtk6-qt-dev libtbb-dev ffmpeg libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libjpeg-dev libjasper-dev libpng++-dev libtiff5-dev libopenexr-dev libwebp-dev libhdf5-dev libpython3.5-dev python3-numpy python3-scipy python3-matplotlib libopenblas-dev liblapacke-dev
```
<br>
<br>
<br>

#### OpenCVのダウンロードとビルド  
　OpenCVはレポジトリに無いのでgithubからダウンロードしてビルドする必要がある。だからx86じゃないマシン、（RaspberryPiとか）でもいけるかも？（未検証）  
<br>
　ダウンロードとビルドのための準備
```bash
$ mkdir opencv && cd opencv
$ wget https://github.com/opencv/opencv/archive/3.3.0.tar.gz
$ tar xvf 3.3.0.tar.gz
$ mkdir opencv-3.3.0/build && cd opencv-3.3.0/build
```
<br>

　CMakeの実行 __（クソ長いけど１つのコマンド！最後の".."も必要！！）__  
```bash
$ cmake -G "Unix Makefiles" --build . -D BUILD_CUDA_STUBS=OFF -D BUILD_DOCS=OFF -D BUILD_EXAMPLES=OFF -D BUILD_JASPER=OFF -D BUILD_JPEG=OFF -D BUILD_OPENEXR=OFF -D BUILD_PACKAGE=ON -D BUILD_PERF_TESTS=OFF -D BUILD_PNG=OFF -D BUILD_SHARED_LIBS=ON -D BUILD_TBB=OFF -D BUILD_TESTS=OFF -D BUILD_TIFF=OFF -D BUILD_WITH_DEBUG_INFO=ON -D BUILD_ZLIB=OFF -D BUILD_WEBP=OFF -D BUILD_opencv_apps=ON -D BUILD_opencv_calib3d=ON -D BUILD_opencv_core=ON -D BUILD_opencv_cudaarithm=OFF -D BUILD_opencv_cudabgsegm=OFF -D BUILD_opencv_cudacodec=OFF -D BUILD_opencv_cudafeatures2d=OFF -D BUILD_opencv_cudafilters=OFF -D BUILD_opencv_cudaimgproc=OFF -D BUILD_opencv_cudalegacy=OFF -D BUILD_opencv_cudaobjdetect=OFF -D BUILD_opencv_cudaoptflow=OFF -D BUILD_opencv_cudastereo=OFF -D BUILD_opencv_cudawarping=OFF -D BUILD_opencv_cudev=OFF -D BUILD_opencv_features2d=ON -D BUILD_opencv_flann=ON -D BUILD_opencv_highgui=ON -D BUILD_opencv_imgcodecs=ON -D BUILD_opencv_imgproc=ON -D BUILD_opencv_java=OFF -D BUILD_opencv_ml=ON -D BUILD_opencv_objdetect=ON -D BUILD_opencv_photo=ON -D BUILD_opencv_python2=OFF -D BUILD_opencv_python3=ON -D BUILD_opencv_shape=ON -D BUILD_opencv_stitching=ON -D BUILD_opencv_superres=ON -D BUILD_opencv_ts=ON -D BUILD_opencv_video=ON -D BUILD_opencv_videoio=ON -D BUILD_opencv_videostab=ON -D BUILD_opencv_viz=OFF -D BUILD_opencv_world=OFF -D CMAKE_BUILD_TYPE=RELEASE -D WITH_1394=ON -D WITH_CUBLAS=OFF -D WITH_CUDA=OFF -D WITH_CUFFT=OFF -D WITH_EIGEN=ON -D WITH_FFMPEG=ON -D WITH_GDAL=OFF -D WITH_GPHOTO2=OFF -D WITH_GIGEAPI=ON -D WITH_GSTREAMER=OFF -D WITH_GTK=ON -D WITH_INTELPERC=OFF -D WITH_IPP=ON -D WITH_IPP_A=OFF -D WITH_JASPER=ON -D WITH_JPEG=ON -D WITH_LIBV4L=ON -D WITH_OPENCL=ON -D WITH_OPENCLAMDBLAS=OFF -D WITH_OPENCLAMDFFT=OFF -D WITH_OPENCL_SVM=OFF -D WITH_OPENEXR=ON -D WITH_OPENGL=ON -D WITH_OPENMP=OFF -D WITH_OPENNI=OFF -D WITH_PNG=ON -D WITH_PTHREADS_PF=OFF -D WITH_PVAPI=OFF -D WITH_QT=ON -D WITH_TBB=ON -D WITH_TIFF=ON -D WITH_UNICAP=OFF -D WITH_V4L=ON -D WITH_VTK=OFF -D WITH_WEBP=ON -D WITH_XIMEA=OFF -D WITH_XINE=OFF -D WITH_LAPACKE=ON -D WITH_MATLAB=OFF ..
```
<br>

　ビルド(時間かかる)&インストール  
```bash
$ make -j8
$ sudo make install
```
<br>
<br>
<br>

#### インストール確認  
　インストールされたか確認をPythonを用いて行う  
```bash
$ python3
```
```python
import cv2
cv2.__version__
```
　ここでインストールしたバージョンが表示されたらインストール完了。  
　(Python終了コマンド)  
```python
quit()
```
<br>
<br>
<br>

* * * 
#### 参考  
https://qiita.com/nakasuke_/items/51664807bdd7794db0da  
<br>

#### 謝辞  
　この記事のもととなったドキュメントの大元の情報 ~~(というかこの記事のコマンドの大半)~~ は某カレー大好き先輩によるものです。  
　僕のOpenCVの環境構築にあたって大変お世話になりました。ありがとうございました。~~またカレー食べたいです。~~  
<br>
<br>
<br>
