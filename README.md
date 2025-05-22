# ROS 2 노드 간 토픽 흐름 정리 
## 1. `camera_publisher_node`
- **처리 로직**:
  - 정해진 시간 간격으로 이미지를 `sensor_msgs/Image` 형태로 `camera/image_raw` 토픽에 전달

- **📤 Publish**: `/camera/image_raw`  
<br>
   
## 2. `vla_node`
- **📥 Subscribe**: `/camera/image_raw`
- **처리 로직**:
  - 미리 입력된 prompt 또는 STT로 받은 명령어를 바탕으로
  - 여러 가능한 시나리오 벡터 중 하나 선택
  - 선택된 결과를 `String` 형태로 `/selected_action` 토픽에 전달
- **📤 Publish**: `/selected_action`  

- **처리 로직2**:
  - 미리 입력된 prompt 또는 STT로 받은 명령어를 바탕으로
  - 로봇을 제어 가능한 연속 벡터를 `/cmd_vel` 토픽에 전달
- **📤 Publish**: `/cmd_vel`  
<br>

## 3. `action_executor_node`
- **📥 Subscribe**: `/selected_action`
- **처리 로직**:
  - `/selected_action` 명령에 맞는 `geometry_msgs/Twist` 메시지 생성
  - `/cmd_vel` 토픽에 전달
- **📤 Publish**: `/cmd_vel`  
<br>

## 4. `omni_controller`
- **📥 Subscribe**: `/cmd_vel`
- **처리 로직**:
  - `/cmd_vel`로 들어온 linear, angular 값을 기반으로 로봇 제어
