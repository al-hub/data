# data

This is my personal data repositiory.

#!/bin/bash
docker_ver=1.15.5-gpu-jupyter

#docker install
if [ "$(which docker)" = "" ]; then
	echo "docker installing..."

    curl -fsSL https://get.docker.com/ | sudo sh
    docker version

    sudo usermod -aG docker $USER
    cat /etc/group | grep docker

    sudo systemctl daemon-reload
    sudo systemctl restart docker
fi

#image install
ver=$(docker images | awk '{print $2}')
if [[ "${ver}" == *"${docker_ver}"* ]];then
    echo "found:"$docker_ver
else
    echo "docker pull tensorflow/tensorflow:"$docker_ver
    docker pull tensorflow/tensorflow:$docker_ver
fi

#run docker
docker run -it \
    -v $(pwd)/..:/tf \
    tensorflow/tensorflow:$docker_ver /bin/bash


#!/bin/bash
docker stop $(docker ps -q)
docker rm $(docker ps -a -q)
docker rmi $(docker images -q)
sudo systemctl stop docker

sudo apt-get purge docker-engine docker docker.io docker-ce docker* containerd.io*
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce docker* containerd.io*

sudo rm -rf /var/run/docker.sock /var/run/dockershim.sock /var/run/docker.pid
sudo rm -rf /var/lib/dockershim /var/run/docker /var/run/dockershim.sock
