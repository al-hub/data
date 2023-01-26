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


```
import numpy as np

#range
#---------------------------------------------------------------
temp = range(10)
list_x = list(range(10))

print('[range],[list]:', temp, list_x)

#list <-> numpy
np_x=np.array(list_x)
list_x=np_x.tolist()

print('[list],[numpy]:', list_x, np_x)


#numpy: random
#---------------------------------------------------------------
size = 10
x = np.arange(size)
#y = np.random.uniform(low=1000, high=1500, size=size)
y = np.random.randint(low=1000, high=1500, size=size)

data_append = np.append(x,y)
print(x,y,data_append)

#numpy: reshape
#https://yganalyst.github.io/data_handling/memo_5/
#x.rehape(2,5)
re_x=x.reshape(-1,1)
re_y=y.reshape(-1,1)

#numpy: insert, concatenate 
#https://velog.io/@dgk089/numpy-%EB%B0%B0%EC%97%B4%EC%9D%98-%ED%96%89%EB%A0%AC-%EC%B6%94%EC%B6%9C-%EC%B6%94%EA%B0%80-%EC%82%AD%EC%A0%9C-%EA%B2%B0%ED%95%A9-%EB%B6%84%ED%95%A0
data_insert = np.insert(re_x,1,re_y,axis=1)
data_concat = np.concatenate([re_x, re_y], axis=1)

#seperate
#x=data_concat.T[0]
#y=data_concat.T[1]
x=data_concat[:,0]
y=data_concat[:,1]

print(re_x, re_y, data_insert, data_concat)
#---------------------------------------------------------------


#regression
#---------------------------------------------------------------
from sklearn.linear_model import LinearRegression
import time
start_time = time.process_time()
model = LinearRegression()
model.fit(x.reshape(-1,1),y)

process_time = int(round((time.process_time()-start_time)*1000))
print("w,b,time :", model.coef_, model.intercept_, process_time )
#---------------------------------------------------------------

#plot
#---------------------------------------------------------------
from matplotlib import pyplot as plt
plt.axis([0,size,0,3000])
plt.title("title")
plt.xlabel("xlabel")
plt.ylabel("ylabel")

plt.scatter(x,y)

yy=model.predict(x.reshape(-1,1))
plt.plot(x,yy,color='red',linewidth=1)

plt.show()
#---------------------------------------------------------------


#file write/read
#---------------------------------------------------------------
with open("temp.txt","w") as wfile:
    wfile.write(str(data_concat))
    wfile.close()

with open("temp.txt","r") as rfile:
   data=rfile.read()
   rfile.close()
   print(data)
#---------------------------------------------------------------
```


### 기본사항  
운 (거리)    
[모니터]소수몽키 10년 2년 스프래드차트: 1~2년후에꺽끼면(-> 5~10년 채권 유리)  
예금(30%), 장기채권(20%), 중기(20%), 단기(20%), 위험(10%, 인버스활용)  
화폐흐름 -> zero sum 이 아닌가?
미국예산  
뉴스에 팔아라  
[모니터]금리 vs 영업이익률  
원자재(리튬, 망간  <- 핵심사항은 무엇인가????)   
배터리 재활용  
자동화 (viT ) 레버+인버스  
10년 국고채 (복리구조)      
K돌파, 델타기본식-원리 이해  
매크로마이크로

해보고싶은것: 전환사채(CB) 투자를 통한 IPO 상장

[AI의물리적이해](https://sites.google.com/site/dspark618/talks?authuser=0)
