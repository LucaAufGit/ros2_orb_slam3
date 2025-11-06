FROM osrf/ros:jazzy-desktop-full-noble

RUN apt update

RUN apt install python3-wheel

WORKDIR /workspaces
RUN git clone https://github.com/stevenlovegrove/Pangolin && \
    cd Pangolin && \
    yes | ./scripts/install_prerequisites.sh recommended && \
    cmake -B build && \
    cmake --build build -j4 && \
    sudo cmake --install build && \
    ldconfig

RUN printf "/usr/local/lib\n" > /etc/ld.so.conf.d/local-lib.conf && ldconfig

RUN echo 'source /opt/ros/jazzy/local_setup.bash' >> ~/.bashrc

RUN ln -s /lib/x86_64-linux-gnu/libopencv_core.so.406 /lib/x86_64-linux-gnu/libopencv_core.so.4.5d