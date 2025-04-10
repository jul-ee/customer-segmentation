# 📋 Customer Segmentation 프로젝트

본 프로젝트는 Kaggle에 공개된 E-Commerce 데이터를 바탕으로

고객의 구매 행동 데이터에 기반해 RFM 분석 및 다양한 피처 엔지니어링을 수행하고, 이를 통해 고객을 정량적으로 세분화한 뒤 클러스터링 및 시각화를 거쳐 인사이트를 도출하는 것을 목표로 합니다.

> 데이터 출처: &nbsp;[Kaggle - E-Commerce Data](https://www.kaggle.com/datasets/carrie1/ecommerce-data)

> 프로젝트 진행 과정: &nbsp;[velog.io/@jul-ee](https://velog.io/@jul-ee/posts)

> 프로젝트 기간: &nbsp;2025.04.09 -

> 🛠️ **Tech Stack**
>
>Data Platform: &nbsp;BigQuery (Google Cloud Platform)<br>
Language: &nbsp;SQL, Python<br>
Data Analysis & EDA: &nbsp;pandas, numpy, Jupyter Notebook<br>
Visualization: &nbsp;matplotlib, seaborn, plotly<br>
Machine Learning:<br>- Preprocessing: &nbsp;`scikit-learn` (`StandardScaler`, `PCA`)<br>- Clustering: &nbsp;`KMeans` (from `sklearn.cluster`)<br>- Outlier Detection: &nbsp;`z-score` (from `scipy.stats`)

<br>

## 📂 사용 데이터셋
- 2009년 12월 1일부터 2011년 12월 9일까지의 영국 기반 온라인 도매 쇼핑몰의 거래 기록 데이터
- 주요 컬럼: `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`

<br>


## 분석 프로세스 Part 1

| 단계 | 내용 | 인사이트 |
|------|------|----------|
| **1. 데이터 불러오기** | BigQuery로 업로드하여 SQL 기반 전처리 수행 | 클라우드 기반 분석 환경 경험 |
| ↓ | |
| **2. 데이터 전처리** | 컬럼 구조 파악, 데이터 타입 확인, 결측치 확인 등 | 누락된 고객 ID와 상품 설명이 다수 존재 |
| ↓ | Description은 동일한 제품이라도 일관되지 않게 작성되어 있어 사전 정제 필요&nbsp;&nbsp;&nbsp; |
| **3. 결측치 제거** | CustomerID, Description 등 중요 컬럼 결측치 제거 | 분석 정확성을 위한 정제 작업 |
| ↓ | |
| **4. 중복값 처리** | 완전 중복된 행 제거 (`CREATE OR REPLACE TABLE ... DISTINCT`) | 거래 데이터 중복으로 인한 분석 왜곡 방지 |
| ↓ | 전체 데이터 중 0.4% 가량이 제품이 아닌 서비스 항목 포함 → 분석에서 제외&nbsp;&nbsp;&nbsp; |
| **5. 오류값 처리** | `UnitPrice = 0`, 비정상 StockCode(BANK CHARGES 등) 제거 | 잘못된 또는 분석에 불필요한 데이터 제거 |
| ↓ | |
| **6. RFM 분석** | 고객별 Recency, Frequency, Monetary 계산 및 테이블 구성 | 고객 가치 기반 분류 가능 |
| ↓ | |
| **7. 추가 feature 추출** | 제품 다양성, 평균 재구매 주기, 취소 비율 등 | 고객 행동 패턴에 대한 이해 |

<br>

## 분석 프로세스 Part 2

| 단계 | 내용 | 인사이트 |
|------|------|----------|
| **8. 이상치 처리** | Z-Score 기반 이상치 탐지 및 제거 | 클러스터 왜곡 방지를 위한 노이즈 제거 |
| ↓ | |
| **9. 변수 간 상관관계 분석** | 피처 간 상관계수 분석 및 다중공선성 확인 | PCA 및 변수 선택 기준 마련 |
| ↓ | |
| **10. 피처 스케일링** | StandardScaler 적용 | 거리 기반 알고리즘(K-Means)을 위한 정규화 |
| ↓ | |
| **11. 차원 축소 (PCA)** | 설명 분산 95% 기준, 6개의 주성분 유지 | 계산 효율 향상 및 시각화 기반 확보 |
| ↓ | |
| **12. K-Means 클러스터링** | K=3 기준 클러스터링 수행 | 자동화된 고객 그룹 분류 |
| ↓ | |
| **13. 클러스터 시각화 및 결과 분석** | 2D/3D PCA 시각화 및 클러스터별 특성 분석 | 각 그룹별 맞춤 마케팅 전략 도출 가능 |


<br>

## 주요 feature

- `recency`  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;고객의 마지막 구매 이후 경과일

- `purchase_cnt`  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;총 구매 거래 횟수

- `item_cnt`  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;구매한 총 제품 수량

- `user_total`  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;총 지출 금액

- `user_average`  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;고객 1회 거래당 평균 지출 금액

- `unique_products`  &nbsp; &nbsp;고객이 구매한 고유 제품 개수

- `average_interval`  &nbsp;평균 구매 간격 (일 단위)

- `cancel_rate`  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;전체 거래 중 취소된 거래 비율


<br>
<br>

## 💡 인사이트

이번 프로젝트에서 가장 큰 인사이트는 고객을 단순히 수치로 분류하는 것이 아니라 고객의 행동 패턴을 맥락 속에서 해석하는 시각을 갖는 것이 데이터 분석의 핵심이라는 사실이었다.

K-Means 클러스터링을 통해 고객을 세 그룹으로 나누었지만 그 자체가 인사이트가 아니라, 진짜 중요한 건 각 군집의 특성을 실제 비즈니스 관점에서 해석하는 일이라는 점을 체감했다.
"구매 빈도는 높지만 취소율이 높은 고객", "구매 간격은 길지만 평균 구매액이 높은 고객"처럼 여러 feature 간의 조합에서 드러나는 행동 유형이 훨씬 더 타겟 마케팅에 적합한 기준이 될 수 있다는 사실을 알게 되었다.

또한 클러스터링과 시각화가 끝이 아니라 데이터를 분류한 뒤, 그 결과를 어떻게 설명하고 전략으로 연결할 수 있을지를 끊임없이 고민해야만  진정한 의미의 데이터 기반 의사결정으로 이어질 수 있음을 실감했다.

본 프로젝트를 통해 '고객 세그먼테이션'이란 단순한 군집화가 아니라, 고객을 이해하고 행동을 해석하는 전략적 기준을 만드는 작업이라는 관점을 얻게 되었다.


<br>
<br>

## 회고

Part1에서는 SQL을 활용해 거래 데이터를 전처리하고, 고객별 RFM(Recency, Frequency, Monetary) 점수를 산출하여 초기 세그먼테이션을 시도했다.
- `CustomerID`, `UnitPrice`, `Description` 등의 결측 및 오류 데이터를 직접 SQL 쿼리로 처리하면서 클린 데이터를 구성하는 데 집중했다.
- 해당 분석은 RFM 및 단일 고객 기준 feature에 기반했기 때문에, 고객의 전체 행동 흐름을 파악하는 데 한계가 있었다.
- 추가적인 feature engineering 및 머신러닝 기반 클러스터링을 통한 고객 세그먼테이션의 필요성을 느꼈다.

Part2에서는 `이상치 제거 → 상관관계 분석 → 피처 스케일링 → PCA → K-Means → 시각화`로 이어지는 전체 분석 프로세스를 구현했다.
- 다양한 **Kaggle 데이터셋**을 기반으로 도메인별 고객 행동 패턴을 파악하는 연습을 지속할 예정이다.
- 시각화 과정에서 축의 범위, 레이블, 투명도 등의 세부 설정이 가독성에 큰 영향을 미친다는 점을 체감했고,  
  더 나은 시각적 표현을 위해 matplotlib, plotly, seaborn의 고급 설정 및 레이아웃 커스터마이징을 학습할 필요성을 느꼈다.


<br>
<br>

## 향후 계획

본 프로젝트를 바탕으로 향후에는 아래와 같은 역량을 확장해 나가고자 한다.

✓ &nbsp;단순히 데이터를 분류하는 데 그치지 않고, “고객 이탈을 줄이기 위해 어떤 세그먼트에 집중해야 하는가?”와 같은 비즈니스 의사결정 가설을 기반으로 분석을 설계하는 훈련을 쌓고자 한다.

✓ &nbsp;`K-Means` 외에도 `k-Medoids`, `DBSCAN` 등  다양한 클러스터링 알고리즘을 적용해 보고, 알고리즘 선택에 대한 근거와 성능 비교 분석을 수행해 보고자 한다.

✓ &nbsp;전처리, 스케일링, 클러스터링 등의 과정을 함수화해 보면서 재사용 가능한 구조로 리팩토링하여 이후 다른 데이터셋에 적용해볼 수 있도록 개선할 예정이다.

✓ &nbsp;분석 결과를 단순히 정적 이미지로 전달하는 데서 나아가 Tableau 등으로 대시보드를 제작하여 비기술 사용자에게도 쉽게 인사이트를 전달할 수 있는 커뮤니케이션 역량을 강화하고자 한다.



<br>

> 본 프로젝트는 실제 이커머스 데이터에 기반한 분석 경험을 통해, 데이터 수집 → 전처리 → 분석 → 시각화의 흐름을 학습하는 데 중점을 두었습니다.
