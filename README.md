# 참고 사이트

https://github.com/micro-ROS/micro_ros_stm32cubemx_utils.git

# micro-ROS 지원보드

https://micro.ros.org/docs/overview/hardware/




# 설치방법

## 1. cubemx혹은 cubd ide 설치

https://www.st.com/en/development-tools/stm32cubeide.html


![image](https://github.com/user-attachments/assets/07262d51-f24e-47bc-ac84-b763acbee4a6)

윈도우즈용으로 설치 Get latest선택

![image](https://github.com/user-attachments/assets/b7351213-8b73-4121-b4d5-4e634c7d745e)

홈페이지 가입 필요(Guest로 다운해도 됨, 다만 이후에 가입이 팔요)

![image](https://github.com/user-attachments/assets/83ba7065-3aec-4dc5-b45d-447b97df9ee4)
## 2. docker 설치( 설치 전 WSL 사용할 수 있어야 함, Windows11의 경우 WSL 설치 방법: Powershell관리자권한으로 열고 wsl --install 커맨드 입력 후 리부팅)

https://docs.docker.com/desktop/setup/install/windows-install/

Docker Desktop for Windows -x86_64 선택

![image](https://github.com/user-attachments/assets/bee806ab-f979-4e42-99b2-a4e09fc96252)

cmd창을 열고 docker 명령어를 입력했을 때 다음과 같은 화면이 출력되면 정상 설치.

![image](https://github.com/user-attachments/assets/17c4f00e-35e0-4164-99b5-9b7848052ef1)


## 2. STM32CubeIDE 실행
![image](https://github.com/user-attachments/assets/0ea72f98-ad49-4437-b4ba-9da2677e9698)
![image](https://github.com/user-attachments/assets/f2a69827-c62a-4a8c-a222-da383a0d0d98)
Board Selector선택

자신의 보드의 Commercial Part No를 입력 보드 이미지를 클릭 후 Next선택
![image](https://github.com/user-attachments/assets/5b065b2c-98ca-4405-a3b8-7b84f9bf0c70)


Project Name을 설정 
![image](https://github.com/user-attachments/assets/516c7b83-fe2d-422a-854f-dcaab1af94ff)


