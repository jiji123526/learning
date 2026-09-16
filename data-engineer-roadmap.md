# Data Engineer (Workforce Solutions, L4) 학습 로드맵

> 대상 공고: Data Engineer, Workforce Solutions – Talent Mobility (SEA104, Bellevue WA) · L4 · ID 10544213
> 기준: 주 10시간 · 약 3개월(13주) · Jiwoo Jeong 프로필 맞춤

---

## 출발점 진단 (현재 위치)

| 요구 역량 | 현재 상태 | 필요 조치 |
|-----------|-----------|-----------|
| SQL | 보유 (프로젝트에서 사용) | ✅ 심화(윈도우 함수·성능 튜닝)만 보완 |
| Python 스크립팅 | 강함 | ✅ 데이터 파이프라인 관점으로 재정렬 |
| 데이터 모델링/웨어하우징 | 부분적 (이벤트 소싱 경험 있음) | ⚠️ 정식 DW 모델링 학습 필요 |
| ETL 파이프라인 | 부분적 (annotation/JSONL 파이프라인) | ⚠️ 정통 ETL/오케스트레이션 학습 |
| 빅데이터 (Spark/EMR/Hive) | 거의 없음 | 🔴 신규 학습 (우대사항, 차별화 포인트) |
| AWS 데이터 서비스 | 얕음 (AWS 툴 언급) | ⚠️ Redshift/S3/Glue 실무 필요 |

**핵심**: 자격요건의 핵심(SQL + Python + 파이프라인 개념)은 이미 상당 부분 보유.
→ 이 로드맵은 "합격선 통과"보다 **차별화(빅데이터 + AWS + DW 모델링)** 에 무게를 둠.

---

## Phase 1 — SQL & 데이터 모델링 심화 (2~3주)

이 role의 1순위 스킬. "쿼리 성능 튜닝"이 직무에 명시됨.

**학습 내용**
- 고급 SQL: 윈도우 함수(`ROW_NUMBER`, `RANK`, `LAG/LEAD`), CTE, 서브쿼리 최적화
- 쿼리 성능 튜닝: 실행 계획(EXPLAIN), 인덱스, 조인 전략
- 데이터 모델링: 정규화 vs 비정규화, **스타 스키마 / 스노우플레이크 스키마**, 팩트/디멘전 테이블
- OLTP vs OLAP 차이, SQL vs NoSQL 선택 기준

**실습**
- 기존 프로젝트(yap.의 D1, Jangoing 인벤토리 데이터)를 **스타 스키마로 재설계**
- 느린 쿼리 하나를 EXPLAIN으로 분석 → 인덱스 추가 후 개선 측정

**추천 리소스**: Mode SQL Tutorial (advanced), *The Data Warehouse Toolkit* (Kimball), LeetCode Database

---

## Phase 2 — ETL & 데이터 파이프라인 오케스트레이션 (3주)

"코드 기반 자동화 파이프라인으로 수백만 건 처리"가 핵심 책무.

**학습 내용**
- ETL vs ELT 개념, 배치 vs 스트리밍
- **Apache Airflow** (업계 표준 오케스트레이터) — DAG, 스케줄링, 의존성, 재시도
- 데이터 품질 검증, 멱등성(idempotency), 백필(backfill)
- 파이프라인 모니터링/트러블슈팅 ("operational/data issue 모니터링" 명시)

**실습**
- 로컬 Airflow에서 "API 추출 → 변환 → DB 적재" DAG 완성
- 강점 활용: NLP annotation 파이프라인을 Airflow DAG로 재구성 → **포트폴리오화**

**추천 리소스**: Airflow 공식 튜토리얼, "ETL with Python" 실습 강의

---

## Phase 3 — 빅데이터 (Spark & 분산 처리) (3~4주)

우대사항이지만 **가장 큰 차별화 포인트**. (Hadoop, Hive, Spark, EMR 명시)

**학습 내용**
- 분산 처리 개념: MapReduce, HDFS, 파티셔닝, 셔플
- **Apache Spark** (PySpark) — DataFrame API, transformation/action, lazy evaluation
- Spark SQL (자격요건의 SparkSQL과 직결)
- Hive 개념, Parquet 등 컬럼형 포맷

**실습**
- PySpark로 대용량 CSV/JSON(수백만 행) 집계·조인 처리
- 로컬 Spark → **AWS EMR**에서 동일 잡 실행 (Phase 4 연계)

**추천 리소스**: *Spark: The Definitive Guide*, Databricks Community Edition(무료)

---

## Phase 4 — AWS 데이터 스택 (2~3주, Phase 3와 병행 가능)

직무가 "AWS에서 처리" 중심이고 EMR이 명시됨. AWS 실무 깊이 강화 필요.

**학습 내용 (데이터 엔지니어링 핵심 서비스)**
- **S3** — 데이터 레이크 저장소, 파티셔닝 전략
- **Redshift** — 데이터 웨어하우스 (DW 모델링과 직결)
- **Glue** — 서버리스 ETL, 데이터 카탈로그
- **Athena** — S3 위에서 SQL 쿼리
- **EMR** — 관리형 Spark/Hadoop
- IAM 기본, 비용 인식

**실습**
- S3 적재 → Glue 카탈로그 → Athena 쿼리로 **미니 데이터 레이크** 구축
- (선택) AWS Certified Data Engineer – Associate 자격증으로 학습 구조화

**추천 리소스**: AWS Skill Builder(무료), Adrian Cantrill / Stephane Maarek 강의

---

## Phase 5 — 통합 포트폴리오 프로젝트 (2주)

이력서/인터뷰에서 결정적 차이를 만드는 단계.

**만들 것: 엔드투엔드 데이터 파이프라인 1개**

```
데이터 소스(API/로그)
   → S3 (raw)
   → Spark/Glue 변환 (수백만 행)
   → Redshift 또는 Parquet (data warehouse, 스타 스키마)
   → Airflow로 오케스트레이션
   → Tableau 대시보드 (이미 보유!)
```

이 한 프로젝트가 공고의 **모든 핵심 책무**(수집→처리→모델링→DW→대시보드→오케스트레이션)를 커버.
기존 강점인 **Tableau**를 마지막 시각화에 붙이면 "지표/리포트/대시보드 소유" 책무까지 연결됨.

---

## 인터뷰 준비 (전 기간 병행)

- **코딩**: Python + SQL 문제 (LeetCode Easy/Medium, 특히 SQL)
- **시스템 디자인**: "데이터 파이프라인/웨어하우스를 어떻게 설계할까" 대비
- **Amazon LP (Leadership Principles)**: 내부 이동이므로 필수. 특히 *Dive Deep, Ownership, Deliver Results* 를 본인 프로젝트 사례(STAR 방식)로 정리

---

## 요약 타임라인 (주 10시간 기준, ~13주)

| 주차 | 집중 영역 |
|------|-----------|
| 1–3 | SQL 심화 + 데이터 모델링 |
| 4–6 | ETL + Airflow |
| 7–10 | Spark / PySpark (+ AWS 병행 시작) |
| 9–11 | AWS 데이터 스택 (S3/Glue/Athena/Redshift/EMR) |
| 12–13 | 통합 포트폴리오 프로젝트 + 인터뷰 준비 |

---

## 핵심 조언

자격요건은 이미 거의 통과 상태(1년+ 경력, SQL, Python, 파이프라인 개념).
진짜 갭은 **① DW 모델링 정식 학습, ② Spark/EMR 빅데이터, ③ AWS 데이터 서비스 실무** 세 가지뿐.
여기에 집중하면 3개월 내 충분히 지원 가능.
