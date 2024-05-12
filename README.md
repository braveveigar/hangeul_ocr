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

![image](https://github.com/braveveigar/hangeul_ocr/assets/139527827/77a65dd7-c051-44a5-baf8-71ca37e45ae1)

---

# 글자 인식 모델

![image](https://github.com/braveveigar/hangeul_ocr/assets/139527827/c38236dc-b77f-4938-8865-efdbecb715cc)


---

# 글자 인식 모델 성능 평가 (Score)

글자를 얼마나 잘 맞추는지가 중요하기에 **Accuracy**를 기준으로 평가

|  | CNN | EfficientNet | ResNet |
| --- | --- | --- | --- |
| 추론 소요 시간 | 43초 | 2분 28초 | 4분 43초 |
| 초성 | 0.688 | 0.9734 | **0.9818** |
| 중성 | 0.8017 | **0.9715** | 0.8999 |
| 종성 | 0.8039 | 0.9802 | **0.9846** |
| 글자 | 0.4437 | 0.9296 | 0.8738 |

## 모델 블렌딩

점수가 비교적 잘 나온 ResNet이랑 EfficientNet을 블렌딩, 최적의 비율을 계산

![image](https://github.com/braveveigar/hangeul_ocr/assets/139527827/db770467-3179-44bb-bd13-6d34c347fc47)


|  | Blended |
| --- | --- |
| 초성 | 0.9877 |
| 중성 | 0.9773 |
| 종성 | 0.9885 |
| 글자 | **0.9564** |

---


# 기술 스택

Numpy, Pandas, Matplotlib, Tensorflow, Keras, OpenCV, Ultralytics, PIL, Vscode
