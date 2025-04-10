# 📋 Customer Segmentation 프로젝트

본 프로젝트는 Kaggle에 공개된 E-Commerce 데이터를 바탕으로 고객들의 구매 행동을 분석하고, RFM 분석 및 추가적인 Feature Engineering을 통해 고객을 다양한 기준으로 분류 및 시각화하는 데 목적이 있습니다.

> Tech Stack: &nbsp;BigQuery(Google Cloud Platform), SQL, Python (Pandas, Jupyter Notebook)

> 데이터 출처: &nbsp;[Kaggle - E-Commerce Data](https://www.kaggle.com/datasets/carrie1/ecommerce-data)

> 프로젝트 진행 과정: &nbsp;[velog.io/@jul-ee](https://velog.io/@jul-ee/posts)

<br>

## 📂 사용 데이터셋
- 2009년 12월 1일부터 2011년 12월 9일까지의 영국 기반 온라인 도매 쇼핑몰의 거래 기록 데이터
- 주요 컬럼: `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`

<br>
<br>

## 분석 프로세스

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

**충성 고객**은 구매 빈도가 높고 재방문 간격이 짧으며 취소율이 낮음

대부분의 고객은 단일 상품만 구매, 소수의 VIP 고객이 높은 금액을 지출

**다양한 상품을 구매하는 고객**은 personalized recommendation의 좋은 대상이 됨

**취소율이 높은 고객**은 서비스 경험에 민감한 경향 등 특정 행동 패턴이 존재할 가능성이 있음



<br>
<br>

## 회고
- 현재 분석은 RFM 및 단일 고객 기준 feature에 집중되어 있다.
- 이후 K-Means 기반 클러스터링, 시각화, 머신러닝 기반 고객 세그먼테이션을 진행할 예정이다.
- 다양한 **Kaggle 데이터셋** 분석을 통해 더 많은 도메인 지식과 경험을 쌓아야겠다.


<br>

> 본 프로젝트는 실제 이커머스 데이터에 기반한 분석 경험을 통해,  데이터 수집 → 전처리 → 분석 → 저장 → 시각화의 흐름을 학습하는 데 중점을 두었습니다.
