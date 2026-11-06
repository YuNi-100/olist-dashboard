# Olist 데이터 분석 대시보드

브라질 이커머스 Olist의 주문, 상품, 리뷰, 판매자 데이터를 분석한 수업 프로젝트입니다. Colab에서 학습한 모델을 파일로 저장한 뒤 Streamlit 대시보드에서 불러왔습니다. 분석 Q&A에서는 사실표와 설명서를 검색해 답에 근거 ID를 붙이고, 실제로 검색된 자료와 답변을 비교했습니다.

## STEP 1~4 실행 순서

| 단계 | 어디에서 실행하나요? | 무엇을 확인하나요? |
| --- | --- | --- |
| STEP 1 · 데이터 준비와 모델 학습 | Colab의 수업용 노트북 | PART 02에서 원본 CSV를 준비하고 PART 04에서 통합 데이터를 만듭니다. PART 06, 07_1, 08, 09에서 배포용 모델을 저장하고 PART 10에서 판매자 표를 저장합니다. |
| STEP 2 · 저장한 모델로 예측 | Colab의 감성 분석 데모 | 01_sentiment_demo_gradio.ipynb에서 07_1 모델을 불러와 리뷰 문장을 판정합니다. STEP 3에 필요한 파일을 만드는 단계는 STEP 1과 아래의 배포 준비입니다. |
| STEP 3 · 대시보드 실행 | Colab에서 ZIP 생성, Windows에서 앱 실행 | 00_prepare_deploy_files.ipynb를 모두 실행하고 olist_deploy_files.zip을 받습니다. 압축을 풀어 data, model, requirements.txt를 앱 폴더에 복사합니다. |
| STEP 4 · 근거가 있는 Q&A | Colab 및 실행 중인 대시보드 | 11_olist_rag_chatbot.ipynb를 실행해 자료 검색을 확인합니다. 대시보드의 분석 Q&A에서 질문하고 답변의 근거 ID를 대조합니다. |

### 1. Colab에서 배포 파일 만들기

1. 각 노트북에서 Google Drive를 연결하고 base_path가 실제 Olist 폴더를 가리키는지 확인합니다. 이 실습에서 확인한 경로는 아래와 같습니다. 폴더를 다른 위치에 뒀다면 본인 경로로 바꿉니다.

~~~python
base_path = "/content/drive/MyDrive/Colab Notebooks/Olist"
~~~

2. 원본 CSV가 준비되면 PART 04를 실행해 Olist/data/olist_master_data.csv를 생성합니다. 배포 준비 노트북이 이 파일을 읽어 주문·상품 Parquet 파일을 만듭니다.
3. PART 06 → 07_1 → 08 → 09 → 10 순서로 실행합니다. PART 8에서 ‘저장 완료’를 확인한 뒤 PART 9로 넘어갑니다. PART 10에서는 seller_table.csv를 쓰는 셀까지 실행합니다. 07_2와 07_3은 비교 실습이며 대시보드에는 07_1 모델을 사용합니다.
4. 00_prepare_deploy_files.ipynb를 모두 실행합니다. 다운로드된 olist_deploy_files.zip을 풀고, 아래 구조에 맞게 파일을 넣습니다. Drive에 올린 파일은 Windows의 앱 폴더로 자동 복사되지 않습니다.

### 2. 내 컴퓨터 폴더 맞추기

이 저장소에는 앱 코드와 실습 기록이 있습니다. data와 model에 들어갈 파일은 수업 자료와 Colab 실행 결과로 준비합니다. GitHub에서 ZIP을 받았다면 압축을 푼 뒤 app.py가 보이는 폴더를 VS Code에서 엽니다.

~~~text
olist-dashboard/
├── app.py                       # 앱 시작 파일, 메뉴 구성
├── requirements.txt             # Python 패키지 목록
├── views/                       # 홈, 데이터 분석, 예측, Q&A 화면
├── utils/
│   ├── data.py                  # 분석 데이터 읽기, 집계 및 그래프
│   ├── models.py                # 저장된 모델 읽기, 예측
│   └── rag.py                   # 자료 검색, 답변 생성, 근거 ID 검사
├── data/                        # 로컬에서 준비
│   ├── orders.parquet            # 주문 분석
│   ├── items.parquet             # 상품 분석
│   ├── seller_table.csv          # 판매자 이탈 예측 입력
│   ├── olist_facts.csv           # Q&A 채널 A
│   ├── olist_model_guide.md      # Q&A 채널 B에 추가
│   └── olist_seller_policy.pdf   # Q&A 채널 C에 추가
└── model/                       # 로컬에서 준비
    ├── part6_recommend.pkl
    ├── part7_1_sentiment_ml.pkl
    ├── part8_seller_segment.pkl
    └── part9_seller_churn.pkl
~~~

규정집 PDF가 배포 ZIP에 빠져 있으면 Drive의 Olist/rag에서 내려받아 로컬 data 폴더에 넣습니다. 규정집은 수업용 가상 규정입니다. data/data 또는 model/model처럼 폴더가 이중으로 들어가지 않도록 확인합니다.

### 3. Windows에서 대시보드 실행하기

VS Code에서 **터미널 → 새 터미널**을 열고, 현재 위치에서 app.py와 requirements.txt가 보이는지 확인합니다. 아래 명령은 프로젝트 폴더 안에 .venv를 만드는 경우입니다.

~~~powershell
Get-ChildItem app.py, requirements.txt
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m streamlit run app.py
~~~

가상환경을 앱 폴더의 **한 단계 바깥**에 이미 만들었다면, 앱 폴더에서 Python 경로를 ..\.venv\Scripts\python.exe로 바꿉니다. 학습 환경과 다른 버전에서 모델을 읽는 문제가 생기면 배포 ZIP의 requirements.txt를 확인합니다.

터미널에 표시된 Local URL을 브라우저에서 엽니다. 기본 주소는 http://localhost:8501 입니다. 처음 실행할 때 Email:이 나오면 Enter를 눌러 건너뛸 수 있습니다. 대시보드를 쓰는 동안 터미널을 열어두고, 종료할 때는 Ctrl+C를 누릅니다. 다음에 켤 때는 Streamlit 실행 명령만 다시 입력합니다.

### 4. 분석 Q&A 실행하기

- **Colab**에서는 11_olist_rag_chatbot.ipynb를 열고, 보안 비밀에 OPENAI_API_KEY를 등록한 뒤 노트북 액세스를 켭니다. 셀 2에서 Drive 연결과 경로를 확인하고, rag 폴더의 파일 3개가 출력되면 나머지 셀을 실행합니다.
- **대시보드**에서는 분석 Q&A를 열고 왼쪽 API 키 입력 칸을 사용합니다. 앱은 로컬 .streamlit/secrets.toml이나 OPENAI_API_KEY 환경 변수도 읽습니다. 키는 저장소나 캡처에 넣지 않습니다.
- 채널 A는 사실표, B는 사실표와 모델 설명서, C는 여기에 실습용 규정집을 더합니다. 답 아래의 ‘찾아온 근거 보기’를 펼쳐 ID와 내용을 확인합니다.

## 실습에서 겪은 오류와 해결 방법

| 화면이나 오류 메시지 | 확인할 위치와 해결 방법 |
| --- | --- |
| KeyError: 'review_fill' | 예전 part8_seller_segment.pkl에 판매자 데이터 표가 들어 있었습니다. PART 8을 모두 실행해 군집 모델, 스케일러, 입력 열, 결측 대체값을 다시 저장합니다. PART 9·10과 배포 준비 노트북에서 새 파일을 사용하고, 로컬 model 폴더의 기존 파일도 교체합니다. |
| FileNotFoundError: orders.parquet | 로컬의 app.py 옆 data 폴더에 orders.parquet와 items.parquet를 넣습니다. 판매자 이탈 화면에는 seller_table.csv도 필요합니다. |
| FileNotFoundError: part9_seller_churn.pkl | 로컬 model 폴더에 STEP 1에서 만든 모델 4개가 모두 있는지 확인합니다. 파일 복사 후 브라우저를 새로고침하고, 모델을 바꿨다면 앱을 종료했다가 다시 실행합니다. |
| Could not open requirements file | 터미널이 압축을 푼 바깥 폴더를 가리키고 있었습니다. app.py와 requirements.txt가 함께 있는 안쪽 폴더로 이동한 뒤 설치합니다. |
| Colab에서 Olist/rag를 찾지 못함 | 노트북 기본 경로에 Data_Analysis가 있었지만 실제 Drive에서는 Colab Notebooks 바로 아래에 Olist가 있었습니다. base_path에서 Data_Analysis를 빼고 셀 2를 다시 실행합니다. |
| 채널 C에서 규정집을 찾지 못함 | olist_seller_policy.pdf를 Colab에서는 Olist/rag에, 대시보드에서는 로컬 data에 배치합니다. |

## 실제로 확인한 챗봇 답

| 질문 | 답과 근거 확인 |
| --- | --- |
| 늦게 도착한 주문의 리뷰 점수는? | 평균 2.57점이 F14의 수치와 일치했습니다. |
| 두 번 이상 구매한 고객의 비율은 몇 %야? | 3.0%가 F07의 수치와 일치했습니다. |
| 배송이 빠르면 고객이 평점을 남길까? | F11과 F17은 배송 기간과 배송 관련 리뷰를 다룹니다. 챗봇은 배송 속도와 평점 작성 여부의 직접적인 관계는 확인할 수 없다고 답했습니다. |
