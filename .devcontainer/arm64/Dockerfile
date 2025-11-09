FROM arm64v8/ros:jazzy-ros-base-noble

RUN apt update

RUN apt install -y ros-jazzy-desktop-full

RUN apt install -y python3-wheel python3-natsort

# Install VNC server and lightweight desktop environment
RUN apt install -y \
    tigervnc-standalone-server \
    tigervnc-common \
    xfce4 \
    xfce4-goodies \
    dbus-x11

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

RUN ln -s /lib/aarch64-linux-gnu/libopencv_core.so.406 /lib/aarch64-linux-gnu/libopencv_core.so.4.5d

# Create VNC startup script
RUN mkdir -p /root/.vnc && \
    printf '#!/bin/bash\n\
unset SESSION_MANAGER\n\
unset DBUS_SESSION_BUS_ADDRESS\n\
export XKL_XMODMAP_DISABLE=1\n\
export XDG_CURRENT_DESKTOP="XFCE"\n\
export XDG_MENU_PREFIX="xfce-"\n\
export XDG_SESSION_DESKTOP="xfce"\n\
\n\
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources\n\
\n\
# Start dbus if not running\n\
if [ -z "$DBUS_SESSION_BUS_ADDRESS" ]; then\n\
    eval $(dbus-launch --sh-syntax)\n\
fi\n\
\n\
# Start XFCE\n\
startxfce4 &\n\
\n\
# Keep the script running\n\
wait\n\
' > /root/.vnc/xstartup && \
    chmod +x /root/.vnc/xstartup

# Set DISPLAY for VNC
ENV DISPLAY=:1