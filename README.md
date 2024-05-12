# 초성, 중성, 종성 분할을 이용한 한글 OCR

[모델 다운로드 링크](https://drive.google.com/file/d/1Xd1Q7TPN8U3soJUOE9ugLOHjlJlF4RBq/view?usp=drive_link)

---

# 프로젝트 소개

## 개요

기존 알파벳이 소문자, 대문자 총 52개인 영어와는 달리 초성 중성 종성을 합쳐서 총 11,172개의 조합을 만들 수 있는 한글의 특성에 맞추어 한글을 더 잘 인식할 수 있는 모델 개발

---

# 모델 기본 설계
![image](https://github.com/braveveigar/hangeul_ocr/assets/139527827/53b6be1d-cded-49ba-be09-60deb569a595)

# 객체 탐색 모델

필요 데이터를 추출 후 Train, Valid으로 분할 후 Yolo V8 을 이용해 학습

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/40925703-15f5-4da8-b7ed-3a58ba53c09e/0d9be991-52fe-45a1-82f9-0cd6f1720875/Untitled.png)

---

# 글자 인식 모델

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/40925703-15f5-4da8-b7ed-3a58ba53c09e/a7debdbe-e672-49b1-9cc1-9e645a7f44c3/Untitled.png)

---

# 글자 인식 모델 성능 평가 (Score)

글자를 얼마나 잘 맞추는지가 중요하기에 **Accuracy**를 기준으로 평가

|  | CNN | EfficientNet | ResNet |
| --- | --- | --- | --- |
| 추론 소요 시간 | 43초 | 2분 28초 | 4분 43초 |
| 초성 | 0.688 | 0.9734 | 0.9818 |
| 중성 | 0.8017 | 0.9715 | 0.8999 |
| 종성 | 0.8039 | 0.9802 | 0.9846 |
| 글자 | 0.4437 | 0.9296 | 0.8738 |

## 모델 블렌딩

점수가 비교적 잘 나온 ResNet이랑 EfficientNet을 블렌딩, 최적의 비율을 계산

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/40925703-15f5-4da8-b7ed-3a58ba53c09e/141e06c1-1fdf-4a43-ab27-fccd4d94897a/Untitled.png)

|  | Blended |
| --- | --- |
| 초성 | 0.9877 |
| 중성 | 0.9773 |
| 종성 | 0.9885 |
| 글자 | 0.9564 |

### 주요 오류

|  | 예측 | 실제 답 |
| --- | --- | --- |
| 초성 | ㅈ
ㅇ
ㄱ
ㄷ | ㄱ
ㅁ
ㅋ
ㅌ |
| 중성 | ㅣ
ㅣ
ㅗ
ㅡ
ㅝ
ㅜ
ㅓ | ㅏ
ㅓ
ㅛ
ㅜ
ㅟ
ㅠ
ㅣ |
| 종성 | ㅁ
ㅇ | ㅇ
ㅁ |

---

# 회고

| Keep | 단순히 한글이 적혀 있는 것을 딥러닝에 쓰는 것이 아니라 특징을 이해한 뒤 특징에 맞춰서 라벨을 분해 시킨 뒤 학습을 돌리고 결과를 합치는 방식이 기존보다 좋은 점수를 얻는 다는 것을 배울 수 있었다. |
| --- | --- |
| Problem | - 초성 중성 종성 세개의 모델로 나눠서 실행해 인식 속도가 느려졌다.
- Yolo 모델과 예측 모델을 합친 뒤 이미지를 분석할 때 시간이 걸렸다. 이런 경우 대규모 작업에서 불리할 것으로 보인다.
- 손글씨 데이터 위주로 학습을해 간판 이미지나 어두운 배경 같은 경우 잘 인식을 하지 못했다. |
| Try | - 모델이 좀 더 효율적으로 결과를 내놓을 수 있게 코드와 모델의 경량화 작업도 신경을 써서 모델 설계를 할 예정이다.
- 데이터를 학습 시킬 때 단순 손글씨 뿐 아니라 간판 데이터도 직접 라벨링을 해 학습을 할 수 있게 준비하는 작업을 할 예정이다. |

---

# 기술 스택

Numpy, Pandas, Matplotlib, Tensorflow, Keras, OpenCV, Ultralytics, PIL, Vscode
