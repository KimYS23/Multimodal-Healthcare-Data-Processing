# Multimodal-Healthcare-Data-Processing
Healthcare &amp; Text Data Preprocessing, De-identification (Regex), and Analysis (MIMIC, PGHD)
보건의료 정형 데이터(EMR, PGHD) 및 비정형 임상 텍스트 데이터의 **전처리, 정규화, 머신러닝 예측 모델링, 설명가능 AI(XAI) 해석**을 수행한 프로젝트 아카이브입니다.

---

### 1. 비정형 임상 텍스트 데이터 정제 및 구조화 (`regex_sample.ipynb`)
* **개요**: 비정형 폐암 병리 진단서(Diagnosis Report) 텍스트에서 핵심 임상 정보를 추출하여 정형 데이터(JSON/Dict)로 변환하는 텍스트 파싱 파이프라인 구현
* **주요 수행 내용**:
  * **정규표현식(Regex) 기반 텍스트 파싱**: 다중 패턴 매칭 및 전처리 함수(`normalize_loc`)를 설계하여 불필요한 기호·여백을 정제하고 장기(Site), 수술 위치, 수술 방법 추출
  * **용어 표준화 및 예외 처리**: 다양한 수술명 표기를 표준 용어(`PROC_ALIASES`)로 매핑하고, 진단 섹션 및 시료 섹션의 다단계 보완 추출 로직을 구축하여 결측(`None`) 최소화

### 2. MIMIC 임상 빅데이터 기반 사망률 및 재원일수 예측 (`Final_MIMIC_data_for final exam.ipynb`)
* **개요**: 중환자실(ICU) 실제 임상·혈액검사 데이터(1,200건, 23개 변수)를 활용한 환자 사망 여부(분류) 및 입원 기간(회귀) 예측 모델링
* **주요 수행 내용**:
  * **데이터 전처리**: 수치형 변수 중앙값 대체(Median Imputation), 범주형 변수 결측 처리 및 원-핫 인코딩(One-Hot Encoding), 타깃 변수 로그 변환(`np.log1p`) 수행
  * **머신러닝 모델 비교 및 평가**: Logistic Regression, Random Forest, XGBoost 모델을 학습·비교하여 사망 예측 **Random Forest AUC 0.865 (Accuracy 77.1%)**, **XGBoost AUC 0.863** 달성
  * **설명가능 AI(XAI) 분석**: `SHAP (TreeExplainer)`를 적용해 혈액검사 수치 등 주요 임상 변수가 사망 확률에 미치는 기여도 시각화 및 해석

### 3. 환자 유래 건강 데이터(PGHD) 시계열 패턴 및 상관 분석 (`pghd_analysis_examples_final.ipynb`)
* **개요**: 50명의 사용자에 대해 14일간 10분 간격으로 수집된 웨어러블 생체·활동 데이터(100,800행) 분석
* **주요 수행 내용**:
  * **시계열 전처리 및 파생변수 생성**: 타임스탬프 기반 일자·시간대별 집계, 수면 효율(Sleep Efficiency) 산출 및 야간 안정시 심박수(Resting HR) 기반 상대적 피로도 점수(Fatigue Score) 모델링
  * **비지도 학습 군집화**: 일일 걸음 수, 칼로리 소모량, 평균 심박수 기반 `K-Means Clustering`을 통해 사용자 활동 패턴 4개 군집 분류
  * **생체 신호 상관관계 검증**: 활동 대사량(MET)과 심박수(`r=0.926`), 걸음 수(`r=0.940`) 간의 높은 양의 상관관계 규명
