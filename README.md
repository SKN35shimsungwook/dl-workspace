# 🧠 dl-workspace

딥러닝을 공부하면서 만든 **Jupyter 노트북 학습 노트 모음** + 실제로 돌아가는
**YOLOv8 마스크 탐지 미니 프로젝트**가 들어있는 개인 워크스페이스예요.

## 구성

### 1) 학습 노트 (Jupyter Notebooks)

기초 → DNN → CNN → 전이학습/이미지 증강 → 객체 탐지 순서로 쌓아온 노트북들이에요.

| 주제 | 노트북 |
|---|---|
| **기초** | `dl01_basic`, `nhp01_basic`, `dl03_single_perceptrone`, `dl03_활성화_함수__초기값할당`, `dl04_활성화함수_python` |
| **DNN (완전연결 신경망)** | `dl02_binaryclassfire`, `dl04_dnn`, `dl04-1_dnn_iris`, `dl05_dnn`, `dl0514`, `dl06_dnn_mnist` |
| **CNN (합성곱 신경망)** | `dl07_can_basic`, `dl08_alexnet`, `dl08_cnn_minist`, `DL08_CNN_minst`, `dl_09_resize`, `dl21_mnist_CNN2_PyTorch` |
| **전이학습 · 이미지 증강** | `dl10_전이학습_vgg19`, `dl13_이미지증강_save`, `dl19_흉부 엑스레이 분석과 데이터 증강하기` |
| **PyTorch 응용** | `dl05_torchvision`, `16_응용_파이토치_날씨_이미지 분류모델` |
| **객체 탐지 (YOLO)** | `dl23_streamlit_yolo_mask`, `dl24_annotation_Roboflow`, `302_YOLO_v8s_Mask_3_Classes_Github_GPU` |
| **기타** | `data00_ML_환경설정`, `dlcma` |

`confusion_matrix.png`, `loss_curve.png`, `misclassified_images.png` 등은 위 노트북들을 돌리면서
나온 학습 결과(혼동행렬, 손실 곡선, 오분류 이미지) 캡처예요.

### 2) YOLOv8 마스크 탐지 앱 (`streamit_app_yolo.py`)

웹캠 영상을 실시간으로 받아서 **마스크 착용 상태(정상 착용/미착용/잘못 착용 등)를 탐지**하는
Streamlit 앱이에요. `302_YOLO_v8s_Mask_3_Classes_Github_GPU.ipynb`에서 학습한 YOLOv8 모델을
가져와서 씁니다.

```bash
streamlit run streamit_app_yolo.py
```

## 기술 스택

| 분야 | 기술 |
|---|---|
| **딥러닝 프레임워크** | PyTorch, torchvision, torchsummary |
| **컴퓨터 비전** | OpenCV, MediaPipe, **YOLOv8**(ultralytics), Roboflow(데이터셋 어노테이션) |
| **웹캠 실시간 스트리밍** | streamlit-webrtc, av |
| **앱/배포** | Streamlit |
| **고전 머신러닝** | scikit-learn, imbalanced-learn, mlxtend, mglearn |
| **한국어 자연어처리** | KoNLPy, mecab-ko, kiwipiepy, kss |
| **자연어처리(범용)** | NLTK, transformers, tiktoken, sentencepiece, wordcloud |
| **데이터 처리/시각화** | pandas, numpy, matplotlib, seaborn, missingno, openpyxl |
| **크롤링/자동화** | selenium, webdriver-manager, BeautifulSoup, pyautogui |
| **패키지 관리** | [uv](https://docs.astral.sh/uv/) (`pyproject.toml` + `uv.lock`) |

## 환경 준비

```bash
uv sync
```

Python 3.12 이상이 필요해요 (`pyproject.toml`의 `requires-python` 기준).

## 코드 구성: `streamit_app_yolo.py`

노트북들과 달리 실제로 돌아가는 유일한 앱 코드라 구조를 짚어두면:

- **`@st.cache_resource`로 YOLOv8 모델 캐싱**: 이 데코레이터가 없으면 Streamlit이 위젯을
  조작할 때마다(리런마다) 모델을 처음부터 다시 메모리에 올려서 매번 몇 초씩 멈추는 것처럼
  느껴짐. 캐시해두면 최초 1회만 로드하고 이후 리런에서는 캐시된 인스턴스를 재사용함.
- **모드 3가지**(사이드바 라디오로 전환): 단일 이미지 업로드 탐지 / 웹캠 실시간 탐지
  (`streamlit-webrtc`) / (필요시 확장 가능한 구조).
- 추론 함수 하나(`results[0].plot()`으로 박스가 그려진 이미지를 바로 받음)를 이미지 모드와
  웹캠 모드가 공통으로 호출해서 로직이 중복되지 않게 함.

## 트러블슈팅

이 저장소는 커밋 메시지가 대부분 날짜/숫자라(`114144`, `23132` 등) 실제 버그 수정 경위를
git 이력에서 뽑아낼 만한 게 없어요. 노트북 개인 학습 기록의 특성상 troubleshooting 섹션은
생략합니다.

---

🤖 이 저장소의 README는 Claude Code와 함께 작성했어요.
