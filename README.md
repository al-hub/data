# data
This is data repositiory for al-hub gits.


#docker 사용법 정리 중  
```
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
```  


#크로링 정리 중([BeatifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/))  
```
#!/usr/bin/env python3
# Anchor extraction from HTML document
from bs4 import BeautifulSoup
from urllib.request import urlopen
with urlopen('https://en.wikipedia.org/wiki/Main_Page') as response:
    soup = BeautifulSoup(response, 'html.parser')
    i = 0
    f = open("list.txt", "w")    
    for anchor in soup.find_all('a'):
        # print(anchor.get('href', '/'))
        data = str(i) + ": " + anchor.get('href', '/')
        f.write(data)
        print(data)
        i+=1
        
    f.close()
```

#이미지 크로링 정리 중([google_images_download](https://pypi.org/project/google_images_download/))  
pip install google_images_download 이후,  
Unfortunately all 20 could not be downloaded because some images were not downloadable. 0 is all we got for this search filter!  
오류 발생 시,  

pip uninstall google_images_download  
git clone https://github.com/Joeclinton1/google-images-download.git  
cd google-images-download && sudo python setup.py install  
pip install git+https://github.com/Joeclinton1/google-images-download.git  


