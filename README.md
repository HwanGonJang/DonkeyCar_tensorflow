# 자율주행 자동차(DonkeyCar) with Raspberrypi, Python
라즈베리파이를 이용한 동키카 인공지능 자율주행 자동차 구축 프로젝트.

## 개요
https://docs.donkeycar.com/guide/build_hardware/
동키카는 라즈베리파이와 RC카, 카메라를 이용하여 학습한 루트를 자율주행으로 주행 할 수 있는 자동차입니다. 기본적으로 라즈베리파이가 RC카를 제어하여 학습한 루트의 모델파일을 파이썬으로 주행하는 원리입니다. 기본적으로 라즈베리파이를 다룰 수 있기 때문에 인공지능을 학습하면서 멋진 결과물을 만들 수 있을 것 같습니다.

## 개발과정
동키카에 필요한 부품들은 모두 따로 구입해야합니다. RC카, 라즈베리파이, 카메라, 자동차의 프레임 및 부속들 등등... 모든 것을 개인이 구매하기엔 가격 부담이 있지만 저는 메이커스페이스에서 조달했기 때문에 만들 수 있었습니다.

동키카의 사이트는 모두 영어이고 제가 개발하는 환경과 조금 다른 부분이 있어서 사이트의 방법과는 조금 다를 수 있습니다.

#### 기술
* Python
   * 루트의 사진들, 주행속도, 바퀴 회전률 등의 json 파일을 종합적으로 학습합니다.
* 라즈베리파이
  * 리눅스 기반의 라즈베리파이로 자동차를 제어합니다.

### 1. 하드웨어 구축
하드웨어 구축은 사이트에 자세히 나와있습니다. 재료를 모두 준비해 구축한 결과물입니다.
<img src='https://github.com/HwanGonJang/HwanGonJang.github.io/blob/master/Pictures/d_1.jpg?raw=true'>

### 2. 소프트웨어 설치(윈도우)
라즈베리파이가 아닌 pc에서 개발환경을 세팅하는 이유은 라즈베리파이로는 인공지능을 학습시키기 버겁기 때문입니다. 동키카는 개발에 필요한 자원들을 모아둔 리포지토리를 깃허브에서 제공합니다. 개발은 아나콘다의 가상환경에서 진행합니다.

~~~c
git clone https://github.com/autorope/donkeycar
cd donkeycar
git checkout master
~~~

깃허브에서 동키카 리포지토리를 클론해줍니다. 

~~~c
conda env create -f install\envs\windows.yml
conda activate donkey
pip install --user tensorflow==2.2.0
pip install -e .[pc]
~~~

동키카를 개발하고 주행하기 위해서는 activate 명령을 실행해주어야 합니다. 그리고 학습에서 필요한 tensorflow를 버전에 맞게 다운로드 해줍니다. 

다음으로 라즈베리파이 설정을 해줍니다. SD카드에 라즈베리파이의 OS인 라즈비안을 넣어줍니다. 라즈비안은 기본적으로 리눅스 시스템입니다. 라즈비안을 다운 받으면 다음으로 윈도우에서 라즈베리파이를 제어하기 위해 설정 파일의 수정이 필요합니다.

~~~c
vi /Volumes/boot/wpa_supplicant.conf
~~~

해당 위치의 파일을 vi로 수정합니다.

~~~c
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="<your network name>"
    psk="<your password>"
}
~~~

나라는 미국의 사이트여서 US이나 크게 상관없습니다. 자신의 네트워크 이름과 패스워드를 써줍니다. 

이후 사이트에서는 ssh와 putty 이용하여 라즈베리파이를 원격으로 제어하는데 저는 UI가 깔끔한 MobaXterm이라는 ssh 프로그램을 사용했습니다. 이 프로그램으로 라즈베리파이와 연결하려면 먼저 라즈베리파이의 ip를 알아야합니다. 이는 Advanced IP Scanner 라는 프로그램으로 해결했습니다. ipconfig 에서 나오는 네트워크의 ip를 이 프로그램에 붙여넣고 스캔 버튼을 누르면 라즈베리파이의 ip가 나옵니다. 

<img src='https://github.com/HwanGonJang/HwanGonJang.github.io/blob/master/Pictures/d_5.jpg?raw=true'>

이 ip를 mobaxterm에서 Sessin-SSH 에 들어가 그대로 입력해줍니다. 그럼 아이디와 패스워드를 설정한 후에 라즈베리파이와 잘 연결되는 것을 볼 수 있습니다.

<img src='https://github.com/HwanGonJang/HwanGonJang.github.io/blob/master/Pictures/d_2.png?raw=true'>

이것으로 윈도우에서 라즈베리파이를 제어할 수 있게 되었습니다.


### 동키카 설정하기
이제 동키카의 설정을 해줄 차례입니다.

~~~python
donkey createcar --path ~/mycar
cd ~/mycar
nano myconfig.py
~~~

위 명령으로 설정 파일을 수정해줍니다. 기본적으로 동키카의 속도와 좌우핸들링 둥 쥬행에 대한 설정입니다. 사이트에 자세히 나와있습니다.
다음은 조이스틱 설정입니다. 조이스틱은 선택이지만 나중에 학습할 떄 조이스틱이 없으면 마우스로 조종을 해야하기 때문에 해줍니다.

~~~python
sudo apt-get install joystick
ls /dev/input/js0
sudo jstest /dev/input/js0
~~~
위 명령어로 조이스틱 조종에 필요한 자원들을 다운 받습니다.


### 동키카 캘리브레이션
~~~python
nano ~/mycar/myconfig.py
~~~

다시 설정 파일로 들어와서 캘리브레이션을 해줍니다. 캘리브레이션은 쉽게말해 초기설정으로 각 사용자가 사용하는 RC카 마다 출력이나 핸들링 정도가 다르기 떄문에 필요한데 주행 설정 값을 바꾸면서 동키카의 최대 핸들 값과 주행속도 값을 맞춰주면 됩니다. 그리고 사이트 처럼 벽에서 동키카를 회전시키면서 아래 사진에 가깝게 값을 조정해주면 됩니다.

<img src='https://github.com/HwanGonJang/HwanGonJang.github.io/blob/master/Pictures/d_6.png?raw=true'>


### 동키카 학습하기
동키카를 주행하기 위해서는 다음 명령을 실행합니다.

~~~python
cd ~/mycar
python manage.py drive
~~~

그럼 자동으로 파이썬이 실행되면서 <your car's hostname.local>:8887 의 주소로 서버가 돌아갑니다. 여기에서 주행을 관리할 수 있습니다. 이제 학습을 위해 Start Recording 버튼을 눌러 카메라를 실행합니다. 이제 원하는 루트를 정해서 약 10~20바퀴 정도 계속 돌려주면 자동으로 카메라가 사진을 찍고 각 사진별로 해당하는 핸들링 정도와 주행속도를 json 파일로 저장합니다.

<img src='https://github.com/HwanGonJang/HwanGonJang.github.io/blob/master/Pictures/d_3.jpg?raw=true'>

루트 주행을 마치면 이제 학습을 시켜주어야 하는데 라즈베리파이에서 돌리기에는 많이 무겁기 때문에 호스트 pc로 파일을 옮겨서 학습한 뒤 학습한 모델 파일(.h5)을 다시 라즈베리파이로 옮겨주어야 합니다. 사이트에서는 rsync 라는 명령어로 이동하는데 저는 이 명령이 듣지 않아서 mobaxterm을 사용했습니다. mobaxterm은 드래그앤드랍으로 파일을 쉽게 옮길 수 있습니다. 

~~~python
donkey train --tub <tub folder names comma separated> --model ./models/mypilot.h5
~~~

학습에 필요한 파일들은 날짜-tub의 이름으로 폴더가 생성되는데 이 안에 있습니다. 폴더이름을 넣어서 위 명령을 해주면 학습을 시작합니다.

학습이 끝나면 다시 드래그앤드롭으로 학습한 파일을 라즈베리파이에 옮겨줍니다.

~~~python
python manage.py drive --model ~/mycar/models/mypilot.h5
~~~

이제 다시 학습한 모델과 함께 드라이브 명령을 실행시켜주면 자율주행 동키카 완성입니다!

## 결과
이것으로 동키카 구축을 마쳤습니다. 제 개발환경과 사이트의 설명이 다른 부분이 많아서 힘들었지만 끝내 성공하니 이 또한 경험으로 남은 것 같습니다. 사이트에서는 절연테이프로 라인을 만들어서 학습했지만 저는 그냥 돌렸는데도 의외로 굉장히 잘 되어서 놀랐습니다. 이렇게 결과로 보고 나니 인공지능에 관련해서 더 공부해봐야 겠다는 생각이 듭니다. 

다음은 시연 영상입니다.
https://youtu.be/dGo2eY3jaSA
