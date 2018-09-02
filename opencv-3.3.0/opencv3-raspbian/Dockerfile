FROM resin/rpi-raspbian:latest

RUN apt-get update
RUN apt-get install -y build-essential cmake cmake-curses-gui pkg-config wget unzip
RUN apt-get install -y libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev libavcodec-dev \
                        libavformat-dev libswscale-dev libeigen3-dev libxvidcore-dev libx264-dev

RUN apt-get install -y libgtk2.0-dev
# Install video4all packages and test
RUN apt-get install -y libv4l-dev v4l-utils
# Install additional packages
RUN apt-get install -y libatlas-base-dev gfortran
# Download OpenCV sources

# openCV ver 3.3.0
RUN cd ~
RUN wget https://github.com/opencv/opencv/archive/3.3.0.zip -O opencv_source.zip
RUN wget https://github.com/opencv/opencv_contrib/archive/3.3.0.zip -O opencv_contrib.zip
RUN unzip opencv_source.zip
RUN unzip opencv_contrib.zip

WORKDIR opencv-3.3.0
RUN mkdir build
WORKDIR  build

# Prepare build
RUN cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D BUILD_WITH_DEBUG_INFO=OFF \
        -D BUILD_DOCS=OFF \
        -D BUILD_EXAMPLES=OFF \
        -D BUILD_TESTS=OFF \
        -D BUILD_opencv_ts=OFF \
        -D BUILD_PERF_TESTS=OFF \
        -D INSTALL_C_EXAMPLES=ON \
        -D INSTALL_PYTHON_EXAMPLES=ON \
        -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.3.0/modules \
        -D ENABLE_NEON=ON \
        -D WITH_LIBV4L=ON \
        ../

# Start build
RUN make -j4

# Install
RUN sudo make install
RUN sudo ldconfig

# This should instead be run after image installation
#RUN modprobe bcm2835-v4l2