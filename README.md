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

