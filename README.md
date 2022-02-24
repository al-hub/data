# data
This is data repositiory for al-hub gits.


## docker 사용법 정리 중  
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

## CSS(Castcading Style Sheets) 이해  
tag속성설정가능 ( h1, span 여러개 등 )  
class인자들속성설정가능 (.사용) - html에서 class는 띄어쓰기로 여러개 줄 수 있음  
html-css link 해 줘야 함 (즉, 다른서버에 이미 만들어 놓은 css 사용할 수도 있음 [bootstrap](https://getbootstrap.com) )   
공개된 코드들  [codepen](https://codepen.io)  
[구름](https://www.goorm.io) 컨테이너 구동 후 동작 [샘플](https://css-rsfra.run.goorm.io/css/index.html)  

## 크로링 정리 중([BeatifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/))  
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

## 이미지 크로링 정리 중([google_images_download](https://pypi.org/project/google_images_download/))  
```
from google_images_download import google_images_download   #importing the library

response = google_images_download.googleimagesdownload()   #class instantiation

arguments = {"keywords":"Polar bears,baloons,Beaches","limit":20,"print_urls":True}   #creating list of arguments
paths = response.download(arguments)   #passing the arguments to the function
print(paths)   #printing absolute paths of the downloaded images
```
pip install google_images_download 이후,  
Unfortunately all 20 could not be downloaded because some images were not downloadable. 0 is all we got for this search filter!  
오류 발생 시,  

pip uninstall google_images_download  
git clone https://github.com/Joeclinton1/google-images-download.git  
cd google-images-download && sudo python setup.py install  
pip install git+https://github.com/Joeclinton1/google-images-download.git  

## [Teachable Machine Learning](https://teachablemachine.withgoogle.com/)   
[tf-rsp.netlify.app](https://tf-rsp.netlify.app)  
