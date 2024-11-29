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

galactic버전 기준으로 다운

https://github.com/micro-ROS/micro_ros_stm32cubemx_utils/tree/galactic

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
docker pull microros/micro_ros_static_library_builder:galactic
```
docker를 통해 컨테이너를 다운받은 후 다음 명령을 통해 해당 라이브러리를 빌드 수행.


```bash
    docker run --rm -v <ABSOLUTE_PATH_TO_PROJECT>:/project --env MICROROS_LIBRARY_FOLDER=micro_ros_stm32cubemx_utils/microros_static_library_ide tjdalsckd/microros_static_library:galactic
```
%% galactic에서는 기존의 docker가 실행되지않아 수정한 도커로 변경

아래는 예시

```bash
docker run --rm -v C:\Users\wdrac\STM32CubeIDE\workspace_1.16.1\microROSTest:/project --env MICROROS_LIBRARY_FOLDER=micro_ros_stm32cubemx_utils/microros_static_library_ide tjdalsckd/microros_static_library:galactic
```

아래와 같이 docker환경에서 빌드를 수행함.

![image](https://github.com/user-attachments/assets/57ba593e-4a1c-4888-ae3a-034273dfe0e2)

완료 후

![image](https://github.com/user-attachments/assets/44af9433-be21-4b52-896b-45fa9aee9bea)



STM32CubeIDE Refresh할 경우 다음과 같이 Project Explorer에 micro_ros_stm32cubemx_utils가 추가됨

![image](https://github.com/user-attachments/assets/ee566e19-36c0-45f2-be60-f75124892ae8)


다음과 같이 microros_static_library_ide/libmicroros가 존재해야 함 (빌드 시 생성됨)

![image](https://github.com/user-attachments/assets/532f0319-89c4-4d3d-8337-80edacc48365)


## 5. C++ 빌드 설정

project Explorer에서 해당 프로젝트 우클릭 후 properties 들어가서 C/C++ Build -> Settings 선택

![image](https://github.com/user-attachments/assets/0ddf0d1e-fb33-4cf8-907a-e13187a38eaf)


Tool Settings-> MCU/MPU GCC Compiler에서 Include paths-> Include Paths 에서  다음을 추가
```bash
../microros_static_library_ide/libmicroros/include
```
![image](https://github.com/user-attachments/assets/28758dbe-b850-4084-8620-106d60a2c4e0)


Tool Settings-> MCU/MPU GCC Linker에서 Libraries에서 다음 사진과 같이 추가

```bash
../microros_static_library_ide/libmicroros
```

![image](https://github.com/user-attachments/assets/cd2f07d3-8b21-4069-96e2-a15fa64cf598)


모두 적용 후 닫기


코드 제너레이션 수행 ctrl+s 누르면 자동

![image](https://github.com/user-attachments/assets/b764c646-45df-418b-a04b-0b5217c3f15e)

micro_ros_stm32cubemx_utils/extra_sources에서  custom_memory_manager.c와 microros_allocators.c, microros_time.c 복사 후 Core/src에 붙여넣기

![image](https://github.com/user-attachments/assets/e1e05ed1-4912-4d3e-8a89-81accd21ef55)

micro_ros_stm32cubemx_utils/extra_sources/microros_transports에서  dma_trasports.c 복사 후 Core/src에 붙여넣기

![image](https://github.com/user-attachments/assets/6106709d-587b-4cd9-9ce4-b0b999c6d3e7)


![image](https://github.com/user-attachments/assets/6e0da8e8-a164-4678-804c-436cd0d58c2c)


![image](https://github.com/user-attachments/assets/468ceaba-851e-47d6-a356-12e2f777feac)



## 6. 예제 코드 작성

src/main.c에서 다음과 같이 변경

```bash
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "string.h"
#include "cmsis_os.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include <rcl/rcl.h>
#include <rcl/error_handling.h>
#include <rclc/rclc.h>
#include <rclc/executor.h>
#include <uxr/client/transport.h>
#include <rmw_microxrcedds_c/config.h>
#include <rmw_microros/rmw_microros.h>

#include <std_msgs/msg/int32.h>
/* USER CODE END Includes */
```



```bash

/* USER CODE BEGIN 4 */
bool cubemx_transport_open(struct uxrCustomTransport * transport);
bool cubemx_transport_close(struct uxrCustomTransport * transport);
size_t cubemx_transport_write(struct uxrCustomTransport* transport, const uint8_t * buf, size_t len, uint8_t * err);
size_t cubemx_transport_read(struct uxrCustomTransport* transport, uint8_t* buf, size_t len, int timeout, uint8_t* err);

void * microros_allocate(size_t size, void * state);
void microros_deallocate(void * pointer, void * state);
void * microros_reallocate(void * pointer, size_t size, void * state);
void * microros_zero_allocate(size_t number_of_elements, size_t size_of_element, void * state);
/* USER CODE END 4 */

/* USER CODE BEGIN Header_StartDefaultTask */
/**
  * @brief  Function implementing the defaultTask thread.
  * @param  argument: Not used
  * @retval None
  */
/* USER CODE END Header_StartDefaultTask */
void StartDefaultTask(void *argument)
{
  /* USER CODE BEGIN 5 */

  // micro-ROS configuration

  rmw_uros_set_custom_transport(
    true,
    (void *) &huart3,
    cubemx_transport_open,
    cubemx_transport_close,
    cubemx_transport_write,
    cubemx_transport_read);

  rcl_allocator_t freeRTOS_allocator = rcutils_get_zero_initialized_allocator();
  freeRTOS_allocator.allocate = microros_allocate;
  freeRTOS_allocator.deallocate = microros_deallocate;
  freeRTOS_allocator.reallocate = microros_reallocate;
  freeRTOS_allocator.zero_allocate =  microros_zero_allocate;

  if (!rcutils_set_default_allocator(&freeRTOS_allocator)) {
      printf("Error on default allocators (line %d)\n", __LINE__);
  }

  // micro-ROS app

  rcl_publisher_t publisher;
  std_msgs__msg__Int32 msg;
  rclc_support_t support;
  rcl_allocator_t allocator;
  rcl_node_t node;

  allocator = rcl_get_default_allocator();

  //create init_options
  rclc_support_init(&support, 0, NULL, &allocator);

  // create node
  rclc_node_init_default(&node, "cubemx_node", "", &support);

  // create publisher
  rclc_publisher_init_default(
    &publisher,
    &node,
    ROSIDL_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Int32),
    "cubemx_publisher");
	uint32_t adcValue ;

  msg.data = 0;

  for(;;)
  {
    rcl_ret_t ret = rcl_publish(&publisher, &msg, NULL);
    if (ret != RCL_RET_OK)
    {
      printf("Error publishing (line %d)\n", __LINE__);
    }

    msg.data++;

    osDelay(1);
  }
  /* USER CODE END 5 */
}

```




위 코드로 수정 후 빌드 빌드 완료시 다음의 화면

![image](https://github.com/user-attachments/assets/c2e231c0-2bf7-4a4e-a7aa-6f1c7f0505a3)


F11을 눌러서 다운로드 후 F8로 디버깅 후 종료



## 7. ROS2에서 실행법

```bash
source /opt/ros/$ROS_DISTRO/setup.bash

# Create a workspace and download the micro-ROS tools
mkdir microros_ws
cd microros_ws
git clone -b $ROS_DISTRO https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup

# Update dependencies using rosdep
sudo apt update && rosdep update
rosdep install --from-paths src --ignore-src -y

# Install pip
sudo apt-get install python3-pip

# Build micro-ROS tools and source them
colcon build
source install/local_setup.bash
```


```bash
ros2 run micro_ros_setup create_agent_ws.sh
ros2 run micro_ros_setup build_agent.sh
source install/local_setup.sh
```

```bash
ros2 run micro_ros_agent micro_ros_agent serial -b 115200 --dev /dev/ttyACM0
```


실행 후 보드의 전원을 껐다 켜야함. 검정색버튼 




