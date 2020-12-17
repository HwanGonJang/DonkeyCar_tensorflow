# DonkeyCar_tensorflow
라즈베리파이와 동키카 플랫폼을 활용한 인공지능 자율주행차 구축(complete)

# 개요      
동키카 플랫폼과 파이썬, ssh 라즈베리파이 통신을 통해 텐서플로우로 학습시킨 길을 자동으로 주행하는 자동차.     
출처: https://docs.donkeycar.com/guide/build_hardware/

## ~Hardware
동키카에 적용가능한 RC카와 캠, 모터 드라이버, 라즈베리파이로 하드웨어 구축
<img width="500" src="https://user-images.githubusercontent.com/33739448/102475142-dc4c5d00-409c-11eb-94a3-97d54a1e6a54.jpg">

## Install SoftWare      
 ### 윈도우 호스트 PC     
 프로젝트 생성, git으로 라이브러리, 소스파일 등 다운로드 후 콘다 가상환경 생성, 라이브러리 설치(tensorflow)       
 
      git clone https://github.com/autorope/donkeycar
      cd donkeycar
      git checkout master
      conda env create -f install\envs\windows.yml     
      conda activate donkey     
      pip install -e .[pc]     
      conda install tensorflow-gpu==1.13.1      
      
### 라즈베리파이
 ssh 설정 후 putty등 응용프로그램 활용하여 라즈베리파이 호스트 pc로 연결     
 
 이 프로젝트에서는 putty 대신 가독성과 파일에 직접 접근 할 수 있는 MobaXterm 프로그램 샤용.
 <img width="500" src="https://user-images.githubusercontent.com/33739448/102475965-e0c54580-409d-11eb-8a68-0a24e2d195c2.PNG">
 
 와이파이 Ip주소와 라즈베리파이 Ip주소 확인     
 
       ipconfig
 이 프로젝트에서는 Ip와 라즈베리파이의 포트 번호를 바로 알 수 있는 AdvancedIPScaaner 사용.
 
 ssh연결을 위해서는 반드시 호스트 pc와 라즈베리파이가 동일한 와이파이에 연결되어야함.
 
## Create your car application       
myconfig.py를 아래 명령어로 실행 후 자동차 속도에 맞춰 캘리브래이션.
 nano myconfig.py
 각 PWM값을 수정하기.
   
## Drive your car     
 아래 명령어로 동키카 드라이빙 파일 실행. 
   cd ~/mycar
   python manage.py drive
 
 https://<192.168.1.n>:8887 주소창에 입력하면 manage.py에서 실행하는 서버로 이동

     


