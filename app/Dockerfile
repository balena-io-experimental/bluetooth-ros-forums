FROM balenalib/up-board-ubuntu:bionic-run

# Setup ROS
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros-latest.list' && \
    apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 && apt update

RUN export DEBIAN_FRONTEND=noninteractive && \
ln -fs /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime

RUN apt install -y ros-melodic-ros-core ros-melodic-joystick-drivers && \
    dpkg-reconfigure --frontend noninteractive tzdata

#################
# Install Systemd
#################
ENV container docker
RUN apt-get update && apt-get install -y --no-install-recommends \
		systemd-sysv \
	&& rm -rf /var/lib/apt/lists/*

# We never want these to run in a container
# Feel free to edit the list but this is the one we used
RUN systemctl mask \
    dev-hugepages.mount \
    sys-fs-fuse-connections.mount \
    sys-kernel-config.mount \

    display-manager.service \
    getty@.service \
    systemd-logind.service \
    systemd-remount-fs.service \

    getty.target \
    graphical.target

COPY systemd/entry.sh /usr/bin/entry.sh
COPY systemd/balena.service /etc/systemd/system/balena.service

RUN systemctl enable /etc/systemd/system/balena.service

STOPSIGNAL 37
ENTRYPOINT ["/usr/bin/entry.sh"]
ENV INITSYSTEM on
######################
# Finish setup systemd
######################

# Setting up the user application
WORKDIR /usr/src/app
COPY start.sh ./

CMD ["bash", "start.sh"]
