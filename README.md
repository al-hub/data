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


## window python  
window [python38-32](https://www.python.org/downloads/windows/)  
C:\python38-32 설치 및 고급환경설정 path설정(win+r, sysdm.cpl)  
vs code cmd로 연결(powershell x) (python38-32 select interpret)  
pip install pywinauto 설치   



## 크레온  
가입 및 plus 설치  
주문 object 사용동의  
주문내역확인 설정 uncheck   
[크레온api]  (https://money2.creontrade.com/e5/mboard/ptype_basic/plusPDS/DW_Basic_List.aspx?boardseq=299&m=9505&p=8833&v=8639)

## slack  
https://slack.com/  로그인 및 본인계정우클릭→Workspace→채널추가  
https://api.slack.com/  봇만들기  
Create an app(From strach) → OAuth & Permissions → Scopes(chat:write) → install to Workspace  → Token값복사

(slack메신저)  봇추가  
#채널명→우클릭→채널세부정보열기→통합→앱추가

(python코드) 메세지보내기  
```
import requests
 
def post_message(token, channel, text):
    response = requests.post("https://slack.com/api/chat.postMessage",
        headers={"Authorization": "Bearer "+token},
        data={"channel": channel,"text": text}
    )
    print(response)
 
myToken = "xoxb-Token값복사"
 
post_message(myToken,"#채널명","hello world")
```

## 변동성돌파  
```
전일변동성*K 

stock  
buy:0.019  
sell:0.269  

etf  
buy:0.019  
sell:0.019  
```

AutoConnection 
```
import requests
import time, os ,ctypes
import win32com.client

from pywinauto import application
from datetime import datetime
#from pywinauto import application

cpStatus = win32com.client.Dispatch('CpUtil.CpCybos')
cpTradeUtil = win32com.client.Dispatch('CpTrade.CpTdUtil')

myToken = "xoxb-"
def post_message(token, channel, text):
    response = requests.post("https://slack.com/api/chat.postMessage",
        headers={"Authorization": "Bearer "+token},
        data={"channel": channel,"text": text}
    )
    print(response)

def dbgout(message):
    """인자로 받은 문자열을 파이썬 셸과 슬랙으로 동시에 출력한다."""
    print(datetime.now().strftime('[%m/%d %H:%M:%S]'), message)
    strbuf = datetime.now().strftime('[%m/%d %H:%M:%S] ') + message
    #slack.chat.post_message('#system-trading', strbuf)
    post_message(myToken,"#system-trading",strbuf)

def printlog(message, *args):
    """인자로 받은 문자열을 파이썬 셸에 출력한다."""
    print(datetime.now().strftime('[%m/%d %H:%M:%S]'), message, *args)

def check_creon_system():
    """크레온 플러스 시스템 연결 상태를 점검한다."""
    # 관리자 권한으로 프로세스 실행 여부
    if not ctypes.windll.shell32.IsUserAnAdmin():
        dbgout('check_creon_system() : admin user -> FAILED')
        return False
 
    # 연결 여부 체크
    if (cpStatus.IsConnect == 0):
        dbgout('check_creon_system() : connect to server -> FAILED')
        return False
 
    # 주문 관련 초기화 - 계좌 관련 코드가 있을 때만 사용
    if (cpTradeUtil.TradeInit(0) != 0):
        dbgout('check_creon_system() : init trade -> FAILED')
        return False

    dbgout('running creon: SUCCESS')
    return True

dbgout('Start Connecting...') 

retry = 5
while retry > 0:     

    os.system('taskkill /IM coStarter* /F /T')
    os.system('taskkill /IM CpStart* /F /T')
    os.system('taskkill /IM DibServer* /F /T')
    os.system('wmic process where "name like \'%coStarter%\'" call terminate')
    os.system('wmic process where "name like \'%CpStart%\'" call terminate')
    os.system('wmic process where "name like \'%DibServer%\'" call terminate')
    time.sleep(5)        

    app = application.Application()
    app.start('C:\CREON\STARTER\coStarter.exe /prj:cp /id:id /pwd:pwd /pwdcert:pwdcert /autostart')
    time.sleep(30)

    if(check_creon_system()): # 크레온 접속 점검
        break;
    
    dbgout('Retry #' + str(6-retry) ) 
    retry-=1

if(retry <= 0):
    dbgout('Check Computer') 

dbgout('Done.') 
```

AutoTrade
```
import requests
import os, sys, ctypes
import win32com.client
import pandas as pd

from datetime import datetime
import time, calendar

myToken = "xoxb-"

def post_message(token, channel, text):
    response = requests.post("https://slack.com/api/chat.postMessage",
        headers={"Authorization": "Bearer "+token},
        data={"channel": channel,"text": text}
    )
    print(response)

#slack = Slacker('xoxb-')
def dbgout(message):
    """인자로 받은 문자열을 파이썬 셸과 슬랙으로 동시에 출력한다."""
    print(datetime.now().strftime('[%m/%d %H:%M:%S]'), message)
    strbuf = datetime.now().strftime('[%m/%d %H:%M:%S] ') + message
    #slack.chat.post_message('#system-trading', strbuf)
    post_message(myToken,"#system-trading",strbuf)

def printlog(message, *args):
    """인자로 받은 문자열을 파이썬 셸에 출력한다."""
    print(datetime.now().strftime('[%m/%d %H:%M:%S]'), message, *args)
 
# 크레온 플러스 공통 OBJECT
cpCodeMgr = win32com.client.Dispatch('CpUtil.CpStockCode')
cpStatus = win32com.client.Dispatch('CpUtil.CpCybos')
cpTradeUtil = win32com.client.Dispatch('CpTrade.CpTdUtil')
cpStock = win32com.client.Dispatch('DsCbo1.StockMst')
cpOhlc = win32com.client.Dispatch('CpSysDib.StockChart')
cpBalance = win32com.client.Dispatch('CpTrade.CpTd6033')
cpCash = win32com.client.Dispatch('CpTrade.CpTdNew5331A')
cpOrder = win32com.client.Dispatch('CpTrade.CpTd0311')  

def check_creon_system():
    """크레온 플러스 시스템 연결 상태를 점검한다."""
    # 관리자 권한으로 프로세스 실행 여부
    if not ctypes.windll.shell32.IsUserAnAdmin():
        printlog('check_creon_system() : admin user -> FAILED')
        dbgout('check_creon_system() : admin user -> FAILED')
        return False
 
    # 연결 여부 체크
    if (cpStatus.IsConnect == 0):
        printlog('check_creon_system() : connect to server -> FAILED')
        dbgout('check_creon_system() : connect to server -> FAILED')
        return False
 
    # 주문 관련 초기화 - 계좌 관련 코드가 있을 때만 사용
    if (cpTradeUtil.TradeInit(0) != 0):
        printlog('check_creon_system() : init trade -> FAILED')
        dbgout('check_creon_system() : init trade -> FAILED')
        return False

    dbgout('running creon: SUCCESS')
    return True

def get_current_price(code):
    """인자로 받은 종목의 현재가, 매도호가, 매수호가를 반환한다."""
    cpStock.SetInputValue(0, code)  # 종목코드에 대한 가격 정보
    cpStock.BlockRequest()
    item = {}
    item['cur_price'] = cpStock.GetHeaderValue(11)   # 현재가
    item['ask'] =  cpStock.GetHeaderValue(16)        # 매도호가
    item['bid'] =  cpStock.GetHeaderValue(17)        # 매수호가    
    return item['cur_price'], item['ask'], item['bid']

def get_ohlc(code, qty):
    """인자로 받은 종목의 OHLC 가격 정보를 qty 개수만큼 반환한다."""
    cpOhlc.SetInputValue(0, code)           # 종목코드
    cpOhlc.SetInputValue(1, ord('2'))        # 1:기간, 2:개수
    cpOhlc.SetInputValue(4, qty)             # 요청개수
    cpOhlc.SetInputValue(5, [0, 2, 3, 4, 5]) # 0:날짜, 2~5:OHLC
    cpOhlc.SetInputValue(6, ord('D'))        # D:일단위
    cpOhlc.SetInputValue(9, ord('1'))        # 0:무수정주가, 1:수정주가
    cpOhlc.BlockRequest()
    count = cpOhlc.GetHeaderValue(3)   # 3:수신개수
    columns = ['open', 'high', 'low', 'close']
    index = []
    rows = []
    for i in range(count): 
        index.append(cpOhlc.GetDataValue(0, i)) 
        rows.append([cpOhlc.GetDataValue(1, i), cpOhlc.GetDataValue(2, i),
            cpOhlc.GetDataValue(3, i), cpOhlc.GetDataValue(4, i)]) 
    df = pd.DataFrame(rows, columns=columns, index=index) 
    return df

def get_stock_balance(code):
    """인자로 받은 종목의 종목명과 수량을 반환한다."""
    cpTradeUtil.TradeInit()
    acc = cpTradeUtil.AccountNumber[0]      # 계좌번호
    accFlag = cpTradeUtil.GoodsList(acc, 1) # -1:전체, 1:주식, 2:선물/옵션
    cpBalance.SetInputValue(0, acc)         # 계좌번호
    cpBalance.SetInputValue(1, accFlag[0])  # 상품구분 - 주식 상품 중 첫번째
    cpBalance.SetInputValue(2, 50)          # 요청 건수(최대 50)
    cpBalance.BlockRequest()     
    if code == 'ALL':
        #dbgout('계좌명: ' + str(cpBalance.GetHeaderValue(0)))
        #dbgout('결제잔고수량 : ' + str(cpBalance.GetHeaderValue(1)))
        #dbgout('평가금액: ' + str(cpBalance.GetHeaderValue(3)))
        #dbgout('평가손익: ' + str(cpBalance.GetHeaderValue(4)))
        #dbgout('종목수: ' + str(cpBalance.GetHeaderValue(7)))
        strbuf = '계좌점검\n'
        strbuf += '계좌명: ' + str(cpBalance.GetHeaderValue(0)) +'\n'
        strbuf += '결제잔고수량 : ' + str(cpBalance.GetHeaderValue(1)) +'\n'
        strbuf += '평가금액: ' + str(cpBalance.GetHeaderValue(3)) +'\n'
        strbuf += '평가손익: ' + str(cpBalance.GetHeaderValue(4)) +'\n'
        strbuf += '종목수: ' + str(cpBalance.GetHeaderValue(7)) +'\n'
        dbgout(strbuf)

    stocks = []
    for i in range(cpBalance.GetHeaderValue(7)):
        stock_code = cpBalance.GetDataValue(12, i)  # 종목코드
        stock_name = cpBalance.GetDataValue(0, i)   # 종목명
        stock_qty = cpBalance.GetDataValue(15, i)   # 수량
        if code == 'ALL':
            dbgout(str(i+1) + ' ' + stock_code + '(' + stock_name + ')' 
                + ':' + str(stock_qty))
            stocks.append({'code': stock_code, 'name': stock_name, 
                'qty': stock_qty})
        if stock_code == code:  
            return stock_name, stock_qty
    if code == 'ALL':
        return stocks
    else:
        stock_name = cpCodeMgr.CodeToName(code)
        return stock_name, 0


def get_current_cash():
    """증거금 100% 주문 가능 금액을 반환한다."""
    cpTradeUtil.TradeInit()
    acc = cpTradeUtil.AccountNumber[0]    # 계좌번호
    accFlag = cpTradeUtil.GoodsList(acc, 1) # -1:전체, 1:주식, 2:선물/옵션
    cpCash.SetInputValue(0, acc)              # 계좌번호
    cpCash.SetInputValue(1, accFlag[0])      # 상품구분 - 주식 상품 중 첫번째
    cpCash.BlockRequest() 
    return cpCash.GetHeaderValue(9) # 증거금 100% 주문 가능 금액


def get_target_price(code):
    """매수 목표가를 반환한다."""
    try:
        time_now = datetime.now()
        str_today = time_now.strftime('%Y%m%d')
        ohlc = get_ohlc(code, 10)
        if str_today == str(ohlc.iloc[0].name):
            today_open = ohlc.iloc[0].open 
            lastday = ohlc.iloc[1]
        else:
            lastday = ohlc.iloc[0]                                      
            today_open = lastday[3]
        lastday_high = lastday[1]
        lastday_low = lastday[2]
        target_price = today_open + (lastday_high - lastday_low) * 0.5
        return target_price
    except Exception as ex:
        dbgout("`get_target_price() -> exception! " + str(ex) + "`")
        return None


def get_movingaverage(code, window):
    """인자로 받은 종목에 대한 이동평균가격을 반환한다."""
    try:
        time_now = datetime.now()
        str_today = time_now.strftime('%Y%m%d')
        ohlc = get_ohlc(code, 20)
        if str_today == str(ohlc.iloc[0].name):
            lastday = ohlc.iloc[1].name
        else:
            lastday = ohlc.iloc[0].name
        closes = ohlc['close'].sort_index()         
        ma = closes.rolling(window=window).mean()
        return ma.loc[lastday]
    except Exception as ex:
        dbgout('get_movingavrg(' + str(window) + ') -> exception! ' + str(ex))
        return None    


def buy_etf(code):
    """인자로 받은 종목을 최유리 지정가 FOK 조건으로 매수한다."""
    try:
        global bought_list      # 함수 내에서 값 변경을 하기 위해 global로 지정
        if code in bought_list: # 매수 완료 종목이면 더 이상 안 사도록 함수 종료
            #printlog('code:', code, 'in', bought_list)
            return False
        time_now = datetime.now()
        current_price, ask_price, bid_price = get_current_price(code) 
        target_price = get_target_price(code)    # 매수 목표가
        ma5_price = get_movingaverage(code, 5)   # 5일 이동평균가
        ma10_price = get_movingaverage(code, 10) # 10일 이동평균가
        buy_qty = 0        # 매수할 수량 초기화

        if ask_price > 0:  # 매도호가가 존재하면   
            buy_qty = buy_amount // ask_price  


        stock_name, stock_qty = get_stock_balance(code)  # 종목명과 보유수량 조회
        #printlog('bought_list:', bought_list, 'len(bought_list):',
        #    len(bought_list), 'target_buy_count:', target_buy_count)     

        if current_price > target_price \
            and current_price > ma5_price \
            and current_price > ma10_price:  

            printlog(stock_name + '(' + str(code) + ') ' + str(buy_qty) +
                'EA : ' + str(current_price) + ' meets the buy condition!`')            

            strbuf='매수타이밍-진입\n'
            strbuf+=stock_name + '(' + str(code) + ') ' + str(buy_qty) +'EA: ' + str(current_price) +'\n'
            strbuf+='current_price: '+ str(current_price) +'\n'
            strbuf+='target_price: '+ str(target_price) +'\n'
            strbuf+='ma5_price: '+ str(ma5_price) +'\n'
            strbuf+='ma10_price: '+ str(ma10_price) +'\n'
            dbgout(strbuf)

            cpTradeUtil.TradeInit()
            acc = cpTradeUtil.AccountNumber[0]      # 계좌번호
            accFlag = cpTradeUtil.GoodsList(acc, 1) # -1:전체,1:주식,2:선물/옵션                
            # 최유리 FOK 매수 주문 설정
            cpOrder.SetInputValue(0, "2")        # 2: 매수
            cpOrder.SetInputValue(1, acc)        # 계좌번호
            cpOrder.SetInputValue(2, accFlag[0]) # 상품구분 - 주식 상품 중 첫번째
            cpOrder.SetInputValue(3, code)       # 종목코드
            cpOrder.SetInputValue(4, buy_qty)    # 매수할 수량
            cpOrder.SetInputValue(7, "2")        # 주문조건 0:기본, 1:IOC, 2:FOK
            cpOrder.SetInputValue(8, "12")       # 주문호가 1:보통, 3:시장가
                                                 # 5:조건부, 12:최유리, 13:최우선 

            # 매수 주문 요청
            ret = cpOrder.BlockRequest() 
            printlog('최유리 FoK 매수 ->', stock_name, code, buy_qty, '->', ret)
            if ret == 4:
                remain_time = cpStatus.LimitRequestRemainTime
                printlog('주의: 연속 주문 제한에 걸림. 대기 시간:', remain_time/1000)
                time.sleep(remain_time/1000) 
                return False

            time.sleep(2)
            printlog('현금주문 가능금액 :', buy_amount)
            stock_name, bought_qty = get_stock_balance(code)
            printlog('get_stock_balance :', stock_name, stock_qty)

            if bought_qty > 0:
                bought_list.append(code)
                dbgout("`buy_etf("+ str(stock_name) + ' : ' + str(code) + 
                    ") -> " + str(bought_qty) + "EA bought!" + "`")


    except Exception as ex:
        dbgout("`buy_etf("+ str(code) + ") -> exception! " + str(ex) + "`")


def sell_all():
    """보유한 모든 종목을 최유리 지정가 IOC 조건으로 매도한다."""
    try:
        cpTradeUtil.TradeInit()
        acc = cpTradeUtil.AccountNumber[0]       # 계좌번호
        accFlag = cpTradeUtil.GoodsList(acc, 1)  # -1:전체, 1:주식, 2:선물/옵션   

        while True:    
            stocks = get_stock_balance('ALL') 
            total_qty = 0 
            for s in stocks:
                total_qty += s['qty'] 
            if total_qty == 0:
                return True
            for s in stocks:
                if s['qty'] != 0:                  
                    cpOrder.SetInputValue(0, "1")         # 1:매도, 2:매수
                    cpOrder.SetInputValue(1, acc)         # 계좌번호
                    cpOrder.SetInputValue(2, accFlag[0])  # 주식상품 중 첫번째
                    cpOrder.SetInputValue(3, s['code'])   # 종목코드
                    cpOrder.SetInputValue(4, s['qty'])    # 매도수량
                    cpOrder.SetInputValue(7, "1")   # 조건 0:기본, 1:IOC, 2:FOK
                    cpOrder.SetInputValue(8, "12")  # 호가 12:최유리, 13:최우선 
                    # 최유리 IOC 매도 주문 요청
                    ret = cpOrder.BlockRequest()
                    printlog('최유리 IOC 매도', s['code'], s['name'], s['qty'], 
                        '-> cpOrder.BlockRequest() -> returned', ret)
                    if ret == 4:
                        remain_time = cpStatus.LimitRequestRemainTime
                        printlog('주의: 연속 주문 제한, 대기시간:', remain_time/1000)
                time.sleep(1)
            time.sleep(30)

    except Exception as ex:
        dbgout("sell_all() -> exception! " + str(ex))


if __name__ == '__main__': 
    try:
        symbol_list = ['A233740','A278240','A419650','A412570']
        bought_list = []     # 매수 완료된 종목 리스트
        target_buy_count = 4 # 매수할 종목 수
        buy_percent = 0.25   

        dbgout('Starting...')
        printlog('check_creon_system() :', check_creon_system())  # 크레온 접속 점검
        
        stocks = get_stock_balance('ALL')      # 보유한 모든 종목 조회
        total_cash = int(get_current_cash())   # 100% 증거금 주문 가능 금액 조회
        buy_amount = total_cash * buy_percent  # 종목별 주문 금액 계산
        printlog('100% 증거금 주문 가능 금액 :', total_cash)
        printlog('종목별 주문 비율 :', buy_percent)
        printlog('종목별 주문 금액 :', buy_amount)
        printlog('시작 시간 :', datetime.now().strftime('%m/%d %H:%M:%S'))


        strbuf='금액점검\n'
        strbuf+='100% 증거금 주문 가능 금액: ' + str(total_cash) +'\n'
        strbuf+='종목별 주문 비율: '+ str(buy_percent) +'\n'
        strbuf+='종목별 주문 금액: '+ str(buy_amount) +'\n'
        strbuf+='시작 시간: '+ datetime.now().strftime('%m/%d %H:%M:%S') +'\n'
        dbgout(strbuf)

        soldout = False

        while True:
            t_now = datetime.now()
            t_9 = t_now.replace(hour=9, minute=0, second=0, microsecond=0)
            t_start = t_now.replace(hour=9, minute=5, second=0, microsecond=0)
            t_sell = t_now.replace(hour=15, minute=15, second=0, microsecond=0)
            t_exit = t_now.replace(hour=15, minute=20, second=0,microsecond=0)

            #t_start = t_now.replace(hour=22, minute=0, second=0, microsecond=0)
            #t_sell = t_now.replace(hour=23, minute=15, second=0, microsecond=0)
            #t_exit = t_now.replace(hour=23, minute=20, second=0,microsecond=0)

            today = datetime.today().weekday()
            if today == 5 or today == 6:  # 토요일이나 일요일이면 자동 종료
                dbgout('주말')
                printlog('Today is', 'Saturday.' if today == 5 else 'Sunday.')
                sys.exit(0)

            if t_9 < t_now < t_start and soldout == False:
                dbgout('거래전-일괄매도')
                soldout = True
                sell_all()

            if t_start < t_now < t_sell :  # AM 09:05 ~ PM 03:15 : 매수         

                for sym in symbol_list:
                    if len(bought_list) < target_buy_count:
                        buy_etf(sym)
                        time.sleep(1)

                if t_now.minute == 30 and 0 <= t_now.second <= 5: 
                    dbgout('working!!!')
                    get_stock_balance('ALL')
                    time.sleep(5)

            if t_sell < t_now < t_exit:  # PM 03:15 ~ PM 03:20 : 일괄 매도
                dbgout('일괄매도 시간')
                if sell_all() == True:
                    dbgout('`sell_all() returned True -> self-destructed!`')
                    sys.exit(0)

            if t_exit < t_now:  # PM 03:20 ~ :프로그램 종료
                dbgout('거래시간 아님')
                sys.exit(0)

            time.sleep(3)


    except Exception as ex:
        dbgout('`main -> exception! ' + str(ex) + '`')
```

https://flyingkiwi.tistory.com/11  
```
import win32com.client
import pandas as pd

cpohlc = win32com.client.Dispatch('CpSysDib.StockChart')


def get_ohlc(code, qty):  # 인자로 받은 종목의 OHLC 가격 정보를 qty 개수만큼 반환한다
    try:
        cpohlc.SetInputValue(0, code)  # 종목코드
        cpohlc.SetInputValue(1, ord('2'))  # 1 기간, 2:개수
        cpohlc.SetInputValue(4, qty)  # 요청개수
        cpohlc.SetInputValue(5, [0, 2, 3, 4, 5])  # 0:날짜, 2~5:OHLC
        cpohlc.SetInputValue(6, ord('D'))  # D:일단위
        cpohlc.SetInputValue(9, ord('1'))  # 0:무수정주가, 1:수정주가
        cpohlc.BlockRequest()
        count = cpohlc.GetHeaderValue(3)  # 3:수신개수
        columns = ['open', 'high', 'low', 'close']
        index = []
        rows = []
        for i in range(count):
            index.append(cpohlc.GetDataValue(0, i))
            rows.append([cpohlc.GetDataValue(1, i), cpohlc.GetDataValue(2, i),
                         cpohlc.GetDataValue(3, i), cpohlc.GetDataValue(4, i)])
        df = pd.DataFrame(rows, columns=columns, index=index)
        return df
    except Exception as ex:
        print('get_ohlc() -> 에러: ' + str(ex))
        return None


def get_kvalue(code):  # 인자로 받은 종목에 대한 20일 average noise ratio.
    try:
        ohlck = get_ohlc(code, 20)
        ohlck['noiseratio'] = 1 - (abs(ohlck['open'] - ohlck['close']) / (ohlck['high'] - ohlck['low']))
        return round(ohlck['noiseratio'].mean(), 2)
    except Exception as ex:
        print('get_kvalue() -> 에러: ' + str(ex))
        return None
```


이슈 
- 시스템  
  - (AT.py) 크레온 인식부   
  - (AC.py) 크레온 retry 자체   
  - [다중접속](https://june98.tistory.com/6) 멀티
  - window 절전 wakeup 자동동작    
  - 부팅 slacker 메세지, 종료 slacker 메세지, 절전모드 slacker 메세지 -> pc_bot  
- 시트  
  - 수수료, 세금 관계 확인  
  - Basic 모델  (working)
  - K 모델
  - 딥러닝 모델 확인 
- 전략
```
  - 돌파는 오롯이 매수모델
  - 장점: Basic통계검증, 일일현금화, 하락장매수x   
  - 단점: 수수료세금, 돌파후실패(즉 매도모델이없어돌파떨어짐대응x), 돌파시점delay시손해
  - (보완)K값: 0.5-1.평균, 일봉해석후대응(back테스트검증필-조합별정리), 누적된일봉대응(평균), 주간누적,월간누적,분기누적 (시스템상오류를 고려한 예외전략 반듯이필요)
  - (보완)쌀사:돌판전예측 최저점계산 (초단위의 기울기값, 돌파까지 남은시간 계산)
  -            최저점부터 현재까지의 누적된(regressor) 기울기로 돌파여부 판단 가능성 있음  
  -            현시점에서 regressor 기울기가 나오면, 남은시간 계산 가능함

               누적시간
  -            ------- = 돌파확률 
  -            전체시간
  -            
  -            전체시간은 알 수 없다...
  -            다만 현재기울기 확실하다면 남은시간을 예측할 수 있다.
  -                현재기울기가 확실하려면 data가 많아야 한다.
  -                data에서 최근일수록 신뢰도가놓다.
  -                누적데이터가 너무많으면 메모리 이슈가 있을 수 있다. -> 누적데이터는 count하자.
  -                que로 최근데이터를 구한다. (size 정해두자, h2)
  -                QUE에 대한 regressor를 구할 수있다. 
  -                전체regressor vs QUEregrssor 크면 확률높아지고, 작으면 낮아진다.
  -                결정순간은 현재가격이 (open가격-h2) 에 진입 시 진행할 수있다.  (h2>=0 h2< H2_DEF, 수수료세금등을 고려 변동성을 생각해서 h2를 구해보자)
  -                
  -                cur_price > open - h2 && 결정의순간
  -                count>h1 &&
  -                QUEregrssor > 전체regressor  결정하자!!
  -                
  -                결정된값은 전체시간이고 누적시간에 근접함으로 돌파확률이 0.999 에 가까운 값이다. 즉 결정하면된다. 
  -                이상적인 경우에는 결국 open 가격이 되어 버림(target=open)으로 h2를 - 값으로 바꾸어서 target값에 가깝게 하여 시뮬레이션해 보도록 한다.
  -                시뮬레이션의 타당성이 확보된다면 시간의 지남에 따라 trading 되는 data값으로 돌파점을 낮춰서 예상 할 수 있는 힘을 가지게 된다.
  - (보완)비파: 쌀사없이도 동작되어야 한다.
  -             h1수수료와 세금이 기본이 되어야 한다.(안보이는 전기료도 있다 - -) 돌파의 힘은 가지고 있을 것이다 
  -             기본전략에서 min,max 실험치는 0.96 ~ 1.04 이다. 
```

## TIPS  
child git commit count  
```
#!/bin/sh
#find_forks.sh
#example: sh find_forks.sh https://github.com/al-hub/clien_rss/network/members | sort -k 1

#or change forks url here and run sh find_forks.sh
forks="https://github.com/INVESTAR/StockAnalysisInPython/network/members"

if [ ! -z "$1" ]
then
       forks=$1
fi

git="https://github.com"
name=`echo $forks | cut -d '/' -f5`

urls=`curl -s $forks | grep $name | grep "class=\"\" href" | cut -d '"' -f4`
for url in $urls
do
        commits=`curl -s $git$url | grep Commits -B 2 | grep strong | sed 's/.*>\(.*\)<.*/\1/'`
        echo $commits" "$git$url
done   
```
