# AI 스피커 신묘한
##### - 운전자의 실시간 모니터링을 통한 게임진행으로 졸음방지 및 사고 예방 -

: 졸음운전으로 인한 교통사고 예방을 위해 개발된 "신묘한"은 AI 기술과 음악 퀴즈 게임을 결합한 졸음 방지 스피커 시스템입니다. 이 작품은 운전자의 눈을 실시간으로 모니터링하여 졸음 상태를 감지하고, 졸음을 깨우기 위해 음악 퀴즈 게임을 진행합니다. 이를 통해 운전자의 졸음을 쫓아주어 졸음 운전으로 인한 교통사고를 예방하는 것을 목표로 합니다.

##### <담당 역할>
- Eye Blink Detection & Raspberry Setting
- 눈 깜박거림 감지를 위한 얼굴 인식 및 Eye detection 모델 개발
- 3초 이상 운전자가 눈을 감고 있을 경우 졸음을 감지하고 음악퀴즈를 실행하는 알고리즘 개발
- 이 모든 동작을 차 안에서 라즈베리에 전원만 공급하면 동작할 수 있도록 실시간 구현



### 배경 및 필요성
1. 고속도로 교통사고의 주요 원인으로 졸음운전이 1위를 기록
2. 한국도로공사의 자료에 따르면 2018년부터 2022년도까지 고속도로 졸음 및 주시 태만으로 인한 사망자 비율은 매년 증가
   
<img src="https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/d96b031d-c206-40c9-ace5-c39ea67fb666" align="center" width=100%/>


-> 운전자의 실시간 모니터링을 통한 게임 진행으로 졸음 방지 및 사고 예방


---
### 시연 영상

https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/5abc2075-3d94-405a-8d11-71474b6917d3

### 부품 리스트
![image](https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/4a4eee8f-770c-4b5d-b9c3-0bb0e11c7d67)

---
### 전체 플로우 차트

<img src="https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/61f248d6-0738-4011-832e-bbbea3b5c7ed" align="center" width=100%/>

<img src="https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/4f005720-028c-4462-942e-f2f761a77a44" align="center" width=100%/>



---
### 소프트웨어 설계

#### 개발 환경
**하드웨어 요구 사항**
- 라즈베리파이4 ( OS: 64-bits)
- USB 마이크
- 스피커

**소프트웨어 요구 사항**
- 운영 체제: Linux
- 개발 언어: Python 3.9.2
- 필요한 라이브러리: Tensorflow 2.8.0, Keras

#### 딥러닝 모델 설계 
![image](https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/f0cb1d7e-cbd9-4f10-9918-f574341c57ba)


![image](https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/ec73757d-f457-41d6-88c1-6c70b0f95dc5)

**딥러닝 학습에 사용된 데이터셋**

눈을 감은 이미지: 1374장
눈을 뜬 이미지: 1500장 
-> 총 데이터셋: 2874장 (Train: 2586장, Validation: 288장)
-> 데이터셋은 왼쪽 눈 이미지만 있었으므로 오른쪽 눈을 감지할 때에는 오른쪽 눈 사진을 좌우 반전시켜 넣어주어야 합니다.

<img src="https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/199297c6-9dd8-4808-a505-087edf5fa8be" align="center" width=50%/>

##### <사용한 모델의 Summary>

총 10개 정도의 레이어로 구성 되어있고, 컨볼루션+맥스풀링 레이어의 조합을 커널수만 확장(32->64->128)하여 3번 진행했습니다. 컨볼루션 3번 진행 후 Flat 레이어로 한 줄로 쭉 펴주는 과정을 진행합니다. 액티베이션 함수는 relu를 사용했습니다. 출력 결과(눈 사진)들은 눈을 뜬 정도의 비율을 0-1 사이의 값을 가지도록 하기 위해 sigmoid 함수를 사용했습니다. 

##### Confusion Matrix를 통한 성능 검증

<img src="https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/391a4678-77c1-4e8d-a4dd-84ef8f0332bd" align="center" width=50%/>

<모델을 통해 예측된 Validation Sets의 Confusion Matrix 결과>

---
### 하드웨어 설계
![image](https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/140564d2-a21b-4e2d-8792-8345ccf1e63f)

<실제 디자인 컨셉 회의 때 사용했던 필기노트의 일부>

![image](https://github.com/hyomin0804/Anti-drowsiness_Speaker/assets/87791247/934c9f64-4694-45b3-afb1-37410fdcf914)

<Auto-CAD로 제작한 설계 도면>

“신묘한”은 영화 “Wall-E”의 로봇을 모티브로 제작되었습니다. 라즈베리 파이의 카메라가 마치 영화에 나오는 로봇의 눈과 비슷한 이미지를 가지고 있어 선정하게 되었습니다. 또한 월-E는 영화에서 사랑스러운 이미지의 로봇으로 묘사되어 많은 사람들에게 호감을 받았습니다. 이와 같은 외형은 사용자들에게 친근한 느낌을 주고 긍정적인 감정을 가지게 할 것으로 생각해 다음과 같은 외관을 채택하게 되었습니다.
