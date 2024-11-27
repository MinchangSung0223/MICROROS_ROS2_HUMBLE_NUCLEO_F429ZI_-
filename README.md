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


Project Name을 설정 후 Finish

![image](https://github.com/user-attachments/assets/516c7b83-fe2d-422a-854f-dcaab1af94ff)

## 3. 세팅
System Core -> SYS -> Timebase Source: TIM1 

![image](https://github.com/user-attachments/assets/b9b6f6c6-ce05-42c0-869f-ceebd93302ec)

System Core -> RCC -> High Speed Clock (HSE) : Crystal/Ceramic Resonator 

![image](https://github.com/user-attachments/assets/34574d15-ad1d-4988-baf5-1ac182f8144a)

Connectivity -> USART3 -> NVIC Settings -> global interrupt  Enabled 체크

![image](https://github.com/user-attachments/assets/2fd4d573-2ba2-466c-8ea1-f2fb5dcae999)

Connectivity -> USART3 -> DMA Settings -> Add Rx , Tx -> Priority Very High, RX의 경우 Mode를 Circular로

![image](https://github.com/user-attachments/assets/665761a8-9a35-40a2-a693-f0ff8e7f9be9)

추가 USART를 사용할 경우 해당 USART 핀을 활성화 후 위의 설정 적용

Middleware and Software -> FREERTOS -> Interface: CMSIS_V2 -> Tasks and Queues -> defaultTask 더블클릭 -> stack size를 3000으로 설정

![image](https://github.com/user-attachments/assets/d74aae8d-8b98-4f4a-8ebe-832cc610c099)

## 4. microROS 다운

humble버전 기준으로 다운

https://github.com/micro-ROS/micro_ros_stm32cubemx_utils/tree/humble

![image](https://github.com/user-attachments/assets/d3b7392d-cc93-42fd-b691-298112712aef)

압축 해제후 폴더명 변경

![image](https://github.com/user-attachments/assets/17a362c4-9015-48a8-92ec-44dbbadf0e1d)

![image](https://github.com/user-attachments/assets/0a9a95e2-fb38-487b-84b1-7b41230a9854)


해당 폴더를 복사
![image](https://github.com/user-attachments/assets/4ed314d6-c560-4db2-8cac-2aed3e616f3f)


STM32CubeIDE에서 생성한 프로젝트의 속성탭 선택

![image](https://github.com/user-attachments/assets/9a22da54-ea90-4416-ae0b-d150e263ebb7)

Location 맨 우측버특을 눌러 해당 프로젝트가 있는 폴더로 이동

![image](https://github.com/user-attachments/assets/7a0ed842-0e95-4eca-b975-4b47ad7e3c6f)

![image](https://github.com/user-attachments/assets/f79eadd2-656a-4c06-b41e-635aef1bc776)


![image](https://github.com/user-attachments/assets/faeac249-f817-44cf-818c-5360aa560b58)

압축 해제한 micro_ros_stm32_cubemx_utils폴더를 해당 프로젝트 폴더 밑에 붙여넣기

![image](https://github.com/user-attachments/assets/ec954c1d-18ec-47a6-968b-3a51c7629569)

폴더 주소를 copy

![image](https://github.com/user-attachments/assets/0d710831-cc90-49c4-a46d-2b3b8b265929)

cmd창을 열어 해당 프로젝트 폴더로 이동

![image](https://github.com/user-attachments/assets/d44bf759-4802-46d8-93f9-badc0f122cd4)

다음 명령어를 입력
```bash
docker pull microros/micro_ros_static_library_builder:humble
```
docker를 통해 컨테이너를 다운받은 후 다음 명령을 통해 해당 라이브러리를 빌드 수행.


```bash
    docker run -it --rm -v <현재 설치된 프로젝트의 절대위치>:/project --env MICROROS_LIBRARY_FOLDER=micro_ros_stm32cubemx_utils/microros_static_library microros/micro_ros_static_library_builder:humble
```

아래는 예시

```bash
    docker run -it --rm -v C:\Users\wdrac\STM32CubeIDE\workspace_1.16.1\microROSTest:/project --env MICROROS_LIBRARY_FOLDER=micro_ros_stm32cubemx_utils/microros_static_library microros/micro_ros_static_library_builder:humble
```

아래와 같이 docker환경에서 빌드를 수행함.

![image](https://github.com/user-attachments/assets/57ba593e-4a1c-4888-ae3a-034273dfe0e2)


