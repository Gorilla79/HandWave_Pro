# HandWave_Pro
![image](https://github.com/user-attachments/assets/d1bc6634-9e8b-4966-97b2-b5f43dbf08ef)

## 프로젝트 배경

- ‘농인’과의 소통 개선
    - 상세 내용
        - 4차 산업혁명을 넘어서 디지털화가 진행되는 시대에서 아직 농인*이 겪는 어려움은 개선되지 못하고 있다. 그중에서도 의사소통에 불편함은 농인뿐만 아니라 대화하는 상대방도 느낀다.
            
            *농인 : 청각애 장애가 있는 사람을 통칭하며, 본 프로젝트에서는 듣지 못하며 말하지도
            못하는 농아인(聾啞人)을 지칭
            
        - 이러한 불편함을 전공 기술과 비교과 프로그램의 교육을 통해 다양한 지문자, 수지&비수지 신호를 머신러닝, 딥러닝 과정을 거쳐 산출된 데이터를 이용하여 농인과의 소통을 개선하고자 프로젝트를 구성해 보았다.

## 프로젝트 수행 내용

- 농인들과의 소통을 개선할 수 잇는 아이디어와 소통 시 발생하는 문제점을 토론
    - 농인들이 의사소통 대상자가 수어를 못 하는 경우일 때 노트나 필기 도구가 없다면 대상자의 입 모양만을 의지해서 언어를 판단 해야하므로 의사소통에 불편함이 생김
- 수어를 이용한 농인과의 의사소통 프로그램을 만들기 위해선 농인의 의사소통 표현법인 수어를 숙지해야 할 필요가 있음
    - 의사소통 표현법
        - 수어 : 지문자 / 수지 & 비수지 신호
        
        > 수어 : 손으로 나타내는 수지 신호
        > 
        > - 수위 -  수어의 어휘를 구성하는 데 있어 손이 작용하는 위치
        > - 수형 - 일상생활 속에서 쉽게 찾아볼 수 있는 것으로 어떤 형상이나 상징과 관련되어 있는 것으로 형상을 구성해 보이는 것과 상징적인 구성을 보여주는 것
        > - 수동 - 손의 움직임
        > - 수향 - 수화자의 신체에 대한 손바닥의 방향과 펼쳐진 손가락 끝의 방향
        > - 체동 - 수화의 기호 중에 수위, 수형, 수동, 수향의 조합만으로는 낱말이 의미하는 뜻을 전할 수 없어 손이나 손가락 이외의 다른 신체부위의 동작까지 취하는 것
        
        > 비수지 신호 : 손동작 이외의 비수지 신호인 얼굴표정, 고개와 머리의 움직임, 입술의 움직임, 눈썹과 눈의 움직임, 어깨와 몸의 움직임 등이 수화와 함께 때로는 단독으로 자연스럽게 나타나는 것
        > 
        
        > 지문자 : 비수지 신호와 같은 단어로 표현하기 곤란한 어휘, 구어에 있는 말 중 수어 어휘가 저장되지 않은 말을 자음과 모음을 통해 나타내는 것
        > 
- OpenCV를 이용하여 캠으로 촬영한 손바닥과 손가락의 마디를 vertex로 표시해주는 ‘Hand Landmark Model’을 사용하여 vertex 사이의 각도를 통해 동작의 형태를 파악하는 방식으로 진행
    - 지문자
        - 지문자(자음, 모음)를 표현할 때는 정적인 상태에서 동작이 이루어짐
        - 우선 지문자 각각에 해당하는 동작을 다양한 변수의 환경에서 실제로 실행하고, 이때 vertex 사이의 각도 값을 저장하여 데이터를 만듬
        - 그리고 캠을 통하여 3초 동안 손바닥과 손가락 사이의 vertex 값을 산출하고, 저장되어
        있던 데이터와 머신러닝 모델(K-최근접 이웃)을 통해 해당 동작과 근접한 동작의 문자를 출력하도록 설계
    - 비수지 신호
        - 비수지 신호는 정적인 상태가 아닌 동적인 상태에서 이루어지는 형태이고 얼굴의 움직임, 팔, 어깨, 몸 등 다양한 부위를 변형시켜 고려해야 할 사항이 많아 어려움을 겪음
        - 그리하여 수위, 수형, 수동, 수향을 복합적으로 고려하는 프로그램를 통해 비수지 신호 구현에 다가가고자 함
            - 수위, 수형, 수동, 수향을 복합적으로 고려하는 프로그램는 자문자 프로그램과 달리 동적인 상태를 고려
            - 그리하여 Open CV를 이용하여서 한 동작을 30초간 촬영 후 해당하는 vertex 사이의 각도를 저장하여 데이터를 만들고, 이후 딥러닝(순환신경망(RNN) 중 하나인 LSTM을 포함한 인공 신경망)을 이용해 학습 시킴
            - 캠을 통해 프레임 단위로 vertex 사이의 각도를 인식하고 2초 동안 학습 데이터와 근사한 값이 있다면 해당 동작을 출력 하도록 설계
        - 또한 체동을 고려한 프로젝트를 설계하기 위해 ‘Pose Estimation Landmark’를 사용하여 상체의 영역까지 확장하였으며 각 구역별(어깨, 팔, 손목) vertex 사이의 각도 데이터를 저장하는 것 외에는 지문자 프로젝트와 동일
        - 마지막으로 비수지 신호를 인식하기 위해 ‘그물망 얼굴 인식’을 사용하여 얼굴의 모션을 추출하는 프로그램을 활용

## 프로젝트 결과

1. [지문자를 판별하는 프로그램] (지문자 분석기, Fingerspelling Analyzer)
    - 웹 캠의 화면이 켜지면서 캠에 보이는 사람의 손을 인식
    - 인식된 손에 vertex들이 표시되고, 지문자에 해당하는 동작을 수행하면 3초 동안 한 번씩 인식하여 저장되어 있던 데이터와 vertex 사이의 각도 근삿값을 측정해 결과를 도출

1. [수위, 수형, 수동, 수향을 복합적으로 고려하는 프로그램] (포괄적 수어 제스처 해석기,  Comprehensive Sign Language Gesture Interpreter)
    - 웹 캠의 화면이 켜지면서 캠에 보이는 사람의 손을 인식
    - 인식된 손에 vertex들이 표시되고, 특정한 동적인 행동을 수행하면 2초 동안 학습 데이터와 vertex 사이의 각도가 근사한 값이 있는지 판별하여 결과를 도출

1. [체동을 고려하는 프로그램] (전신 수어 해석기, Full-Body Sign Language Interpreter)
    - 웹 캠의 화면이 켜지면서 캠에 보이는 사람의 손, 상체, 얼굴을 인식
    - 인식된 부위에 vertex들이 표시되고, 특정한 동작을 수행하면 3초 동안 한 번씩 인식하여 저장되어 있던 데이터와 각 구역별(어깨, 팔, 손목) vertex 사이의 각도 근삿값을 측정해 결과를 도출

## 기대효과

- 농인이 사용하는 수어는 국가별로 다르므로 데이터만 충분히 학습시킨다면 다양한 언어를 번역할 수 있을 것이다.
- 또한 스마트 글래스에 적용 해당 프로그램을 적용한다면 농인과의 양방향 소통을 크게 개선할 수 있을 것
- 해당 프로젝트의 프로그램 측면에서 보았을 때 농인과의 의사소통 개선 뿐만아니라, 모션을 통한 IOT기기의 양방향 소통 및 제어, VR&XR에서의 활용 등 다양한 방면으로 활용이 가능할 것

------

## 실행 시 

- pip install PyQt5
- pip install opencv-python mediapipe numpy keyboard pillow PyQt5
