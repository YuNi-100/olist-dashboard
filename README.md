# Olist 데이터 분석 대시보드

브라질 이커머스 Olist의 주문, 상품, 리뷰, 판매자 데이터를 분석한 수업 프로젝트입니다. Colab에서 학습한 모델을 Streamlit 대시보드에 연결하고, 분석 자료를 근거로 답하는 RAG 챗봇을 확인했습니다.

| 단계 | 실습 내용 |
| --- | --- |
| STEP 1 | Colab에서 데이터 분석 및 상품 추천, 리뷰 감성 분석, 판매자 유형 분류, 판매자 이탈 예측 모델 학습 |
| STEP 2 | 저장한 감성 분석 모델을 불러와 입력 문장의 긍정·부정 예측 |
| STEP 3 | 배포용 데이터와 모델을 준비하고 Python 3.12와 Streamlit으로 로컬 대시보드 실행 |
| STEP 4 | 사실표, 모델 설명서, 실습용 규정집으로 Q&A를 진행하고 답변의 근거 ID 확인 |

## 챗봇 검증

| 질문 | 답변 검증 |
| --- | --- |
| 늦게 도착한 주문의 리뷰 점수는? | 평균 2.57점이 근거 F14와 일치했습니다. |
| 두 번 이상 구매한 고객의 비율은? | 3.0%가 근거 F07과 일치했습니다. |
| 배송이 빠르면 고객이 평점을 남길까? | F11과 F17을 인용했지만, 배송 속도와 평점 작성 여부의 직접적인 관계는 확인할 수 없다고 답했습니다. |

## 로컬 실행

Windows에서 저장소 폴더를 연 뒤 다음 명령을 실행합니다.

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m streamlit run app.py
```

브라우저에서 http://localhost:8501 을 엽니다. 홈 화면 외 기능은 수업 실습에서 생성한 아래 파일이 있어야 작동합니다. 배포 ZIP의 데이터와 모델을 `app.py` 옆에 배치합니다. 이 저장소에는 코드와 실습 기록을 담았습니다.

| 폴더 | 로컬에 준비할 파일 |
| --- | --- |
| `data/` | `orders.parquet`, `items.parquet`, `seller_table.csv`, `olist_facts.csv`, `olist_model_guide.md`, `olist_seller_policy.pdf` |
| `model/` | `part6_recommend.pkl`, `part7_1_sentiment_ml.pkl`, `part8_seller_segment.pkl`, `part9_seller_churn.pkl` |

분석 Q&A는 대시보드의 비밀번호 입력 칸이나 로컬 Streamlit 비밀 설정을 통해 OpenAI API 키를 사용합니다. 실습용 규정집은 수업 자료입니다.
