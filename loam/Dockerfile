FROM ros:melodic

RUN apt-get update && apt-get install -y --no-install-recommends && apt-get install -y --no-install-recommends wget nano build-essential ros-melodic-pcl-ros
RUN apt-get install -y --no-install-recommends libomp-dev

RUN apt-get install -y --no-install-recommends python3-pip
RUN pip3 install -U pip setuptools
RUN pip3 install numpy optuna evo pyquaternion
RUN pip3 install pyquaternion

RUN apt-get install -y --no-install-recommends unzip

RUN mkdir -p /root/catkin_ws/src
WORKDIR /root/catkin_ws/src
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin_init_workspace'

RUN sed -i '9c #include "lz4.h"' /usr/include/flann/util/serialization.h
RUN sed -i '10c #include "lz4hc.h"' /usr/include/flann/util/serialization.h

COPY loam_velodyne /root/catkin_ws/src/loam_velodyne/
# RUN git clone https://github.com/laboshinl/loam_velodyne

WORKDIR /root/catkin_ws
RUN /bin/bash -c '. /opt/ros/melodic/setup.bash; catkin_make -DCMAKE_BUILD_TYPE=Release'

RUN sed -i "6i source \"/root/catkin_ws/devel/setup.bash\"" /ros_entrypoint.sh


RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
