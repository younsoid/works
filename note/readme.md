# 연구 정리

### 1 데이터

1-1 코호트
- TCGA LUAD·LUSC, DX 슬라이드와 RNA를 모두 가진 905명(LUAD 452, LUSC 453)
- 분석 슬라이드 895장(LUAD 442, LUSC 453):환자당 1장, 특수 염색 1장 제외

1-2 MFP 29개 시그니처
- Bagaev 2021 방법으로 직접 계산, 공개 점수와 Spearman 중앙값 0.991

1-3 MFP 아형(IE, IE/F, F, D)
- 폐암을 뺀 23개 암종 7,031명 참조로 KNN(k = 35), 원 라벨과 일치 84.9%(κ 0.791)

1-4 외부 검증 지표
- ABSOLUTE 순도, Thorsson 2018(백혈구·기질 비율, TIL Regional Fraction), 면역 점수(ESTIMATE, MCP-counter)

### 2 슬라이드 처리

2-1 타일링
- 0.5 mpp 256 px, CLAM 조직 마스크

2-2 타일 품질 관리
- 펜 10%, 어두운 화소 50%, 단색 분산 67, 밝은 화소 90% 기준 → 1,026.6만 타일(통과율 98.4%)

2-3 특징 추출
- EXAONEPath 1.0(ViT-B/16, 768차원), Macenko 정규화, 1,025.3만 타일
- 슬라이드당 타일 중앙값 10,295/11,192

### 3 교차검증 설계

3-1 5-fold
- TSS(기관) 단위 분할
-  test와 학습 사이 TSS 겹침 0
-  fold마다 test 83–94/88–95명, train 258–293명, val 62–90명
-  LUAD fold 2는 test의 98%가 한 기관(TSS 55)

3-2 반복
- fold마다 seed 3개
- 주 비교는 3-seed 평균 예측, 실행 간 순위 비교는 seed 0 단일 모델

### 4 head 클러스터

4-1 주 Head 4개
- G0: 시그니처마다 헤드 29개
- G2: Bagaev 기능 4그룹
- G6: RNA 공발현 군집 k = 4
- G7: G0 attention 형태 유사도 군집 k = 4
- G9: RNA 변동 축

4-2 보조 Head
- G1(헤드 하나)
- GR(G2와 같은 크기의 무작위 묶음 20세트)

4-3 각 Head의 성질
- 결속(gap): G6 0.36/0.36 > G2 0.21/0.22 > GR −0.05~0.05
- fold 간 안정성(ARI): G6 0.97/0.87, G7 0.49–1.00/−0.06–0.58

### 5 보조 오믹스 

5-1 보조 오믹스
- 헤드마다 메틸화와 RPPA 중 내부 교차검증 R²가 큰 층 하나를 고름(학습 fold PCA 20성분, 릿지, R² ≥ 0.10)
- 교차 적합으로 만든 보조 정답을 손실에 λ 0.5로 더함

5-2 비교 설계 
- {G0, G2, G6, G7} × {없음, 다른 오믹스}

5-3 민감도 분석
- λ 0.25·1.0, 시그니처 단위 층 선택

### 6 모델과 학습

6-1 모델
- 그룹마다 attention 헤드를 두는 ABMIL. 29개 시그니처를 회귀(MSE)

6-2 학습 설정
- AdamW, lr 1e-4, wd 1e-4(G0 val 손실로만 탐색해 정함) / val 손실 조기 종료 / 고정 epoch 5회 학습(attention 민감도용)

6-3 기준선
- 슬라이드 평균 특징 + 릿지

6-4 실행
- 총 ?회(주 비교·보조·민감도 ?회)

### 7 평가

7-1 성능 지표
- 시그니처별 Pearson r
- 주 지표: 모든 fold의 test 환자를 합친 r
- 2차 지표: fold 평균 r. 둘의 차이는 기관 간 보정 성분

7-2 판정
- G0 대비 Δr, 환자 부트스트랩 1,000회 95% CI, 시그니처 FDR 0.05
- 동등성: 90% CI가 ±0.02 안
- 그룹 내용 효과: G2의 GR 20세트 안 순위 p < 0.05

7-3 아형 재현: 예측 29개 → 같은 KNN 절차. 정확도, 아형별 재현율

7-4 attention 지표
- 세기와 무관한 지표만: 상위 10% 겹침, 순위 상관
- 설계 간 비교는 다른 seed끼리
- 주 지표: seed 간 재현성으로 나눈 보정 일치도(변화량 = 1 − stay*, 기준 0.05, CI 상한 < 1)

7-5 슬라이드 단위 변화량
- (두 설계의 seed 간 ρ 평균) − 설계 간 ρ

### 8 조직 분석

8-1 대상 칸
- attention이 바뀌고 성능이 오르거나 변화 없음, G0 seed 간 ρ ≥ 0.1, 슬라이드 중앙값 > 0
- 47칸, 대상마다 상위 20장 + 대조 10장

8-2 타일 집합
- gained·lost·kept(seed 평균 상위 10%) 각 50개, 모든 슬라이드에 random 200개, 총 346,489타일

8-3 핵 분할
- HoVer-Net fast PanNuke(TIAToolbox 1.6), 0.25 mpp, fp16(fp32와 일치 0.9999)

8-4 공간 표현형
- 구획(종양/기질) × 면역 밀도 기준값(fold별 67% 분위) → inflamed / excluded / desert

8-5 가설과 판정 기준 (TODO)

### 9 결과

#### R1 학습 양상

R1-1 모든 설계가 epoch 2–3에 val 최저점에 닿음: train 손실은 0.04까지 내려가고 val은 정체(외우기)

R1-2 설계 간 최저 val 손실 차이는 표준오차 1개 안팎(LUAD 0.842–0.857, LUSC 0.885–0.893)

R1-3 고정 epoch 학습(5회, epoch 0–4)의 끝이 모든 설계의 바닥 구간 안

#### R2 성능

R2-1 기본 성능
- G0 3-seed 0.404/0.365
- 기준선 0.357/0.327(단일 모델끼리는 G0와 같거나 낮음)

R2-2 seed 평균 효과(+0.05)가 어떤 설계 효과(최대 +0.024)보다 큼

R2-3 LUAD 묶기 효과
- G7·G9 +0.023, G2·G6 +0.016 / 오른 것은 주로 림프구 계열 시그니처 / GR 20세트도 같은 범위

R2-4 LUSC 묶기 효과
- G2·G6·G7은 동등 / G1는 손해

R2-5 Δr 분해: LUAD 단일 모델 이득의 대부분이 기관 간 보정 성분이고, GR도 같음

R2-6 그룹 내용 효과 없음: GR 순위 p 0.29/0.62

R2-7 묶이면 손해 보는 시그니처: B_cells(LUAD G6 −0.077, G7+다른 오믹스), MDSC_traffic(LUSC)

R2-8 기능 그룹·G6 무리 층화: 이득과 손해가 나는 모양이 GR과 같음

R2-9 보조 오믹스 효과: 모든 비교에서 동등, 유의한 시그니처 0개

R2-10 아형 재현
- 정확도 0.44–0.50 / 0.48–0.51, IE/F 재현율이 최저(0.16–0.24)
- 묶기가 예측의 유효 차원을 줄임

R2-11 강건성·독립 지표·기관 신호·학습 자료 크기 곡선

#### R3 묶음의 구조

R3-1 공발현은 큰 축 하나가 지배(실루엣 k = 2, [28, 1]). 떨어진 하나는 증식

R3-2 G6 k = 4: 면역/기질/호중구 계열/증식, 네 아형이 면역 × 기질 2×2로 갈림

R3-3 G2 친종양·악성세포 특성은 공발현으로 보면 그룹이 아님

R3-4 G7: 큰 무리 하나 + 단독 시그니처(B_cells, 호중구, 혈관 내피)

R3-5 attention 형태 유사도와 RNA 공발현은 함께 가지만(ρ 0.40–0.71) 이탈 쌍이 있음(호중구, 증식)

R3-6 G8 gate는 무너지지 않지만 seed 간 재현되지 않음 → G1 + 시그니처별 가중치로 읽음

#### R4 HEAD 클러스터가 attention을 바꾸는가

R4-1 대부분 그대로(보정 일치도 중앙값 0.94–0.96 / 0.87–0.92)

R4-2 바뀌는 것은 G0에서 따로 보던 시그니처, 그리고 대부분 공통 쪽으로

R4-3 헤드 크기 규칙: 헤드를 같이 쓰는 시그니처가 많을수록 더 바뀜

R4-4 기질 헤드가 혈관 시그니처를 CAF·Matrix 시야로 끌어감

#### R5 보조 오믹스가 attention을 바꾸는가

R5-1 대부분 그대로(변화한 칸 LUAD 11/232, LUSC 5/232)

R5-2 LUAD에서 가장 안 보이던 시그니처만 바뀌고 seed 간 재현성이 오름(Endothelium, 호중구, B_cells, Granulocyte_traffic)

R5-3 묶기에 더해 주는 효과는 작음, 성능 이득 없음

#### R6 바뀐 곳은 어떤 조직인가

R6-1 슬라이드 단위 변화는 MFP 아형과 무관(예외 LUSC B_cells)

R6-2 성능이 오른 칸은 변화가 작고, 크게 바뀐 칸은 성능이 그대로임

R6-3 상위 슬라이드는 타일 단위에서도 다른 곳을 봄: 상위 10% 겹침 0.066 대 대조 0.350(LUAD), 0.042 대 0.278(LUSC)

R6-4 바뀐 타일의 세포 구성(C0–C4) (05 진행 중)

R6-5 헤드 구획 비중 × 공간 표현형(B1–B4) (계획)

#### R7 공간 표현형과 오믹스

R7-1 표현형 판정 검증(TIL, 기질 비율, 순도) (05 대기)

R7-2 MFP 아형 × 표현형(A1·A2) (05 대기)

R7-3 같은 아형 안 표현형 간 오믹스 차이 (계획)

# 논문 리뷰

1. A Pathway-Structured Privileged Distillation Framework for Deployable Computational Pathology [#](https://github.com/younsoid/works/blob/main/note/paper1.md)

# 결과 정리

### 1. 데이터 준비

1-1. 코호트 구성 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note12.md)

1-2. MFP 29 signature 재현 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note13.md)

1-3. MFP subtype 재현 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note13.md)

1-4. MFP score 보조 데이터 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note13.md)

1-5. 보조 오믹스 [#](https://github.com/younsoid/works/blob/main/note/note2.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note14.md)

1-6. 보조 오믹스별 정보량 [#](https://github.com/younsoid/works/blob/main/note/note2.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note16.md)

### 2. H&E slide 처리

### 3. Slide tiling과 QC

### 4. Exaonepath feature extraction

### 5. Hovernet 핵분할

### 6. Grouping과 보조오믹스

6-1. grouping과 비교 설계 [#](https://github.com/younsoid/works/blob/main/note/note3.md)

6-2. g6 grouping 확인 [#](https://github.com/younsoid/works/blob/main/note/note3.md)

6-3. k=2 g6는 사실상 g1이다. [#](https://github.com/younsoid/works/blob/main/note/note4.md)

6-4. k=4 g6 grouping [#](https://github.com/younsoid/works/blob/main/note/note4.md)

6-5. k=4 g6 grouping (2) [#](https://github.com/younsoid/works/blob/main/note/note5.md) (3) [#](https://github.com/younsoid/works/blob/main/note/note9.md)

6-6. 각 grouping의 결속 측정 [#](https://github.com/younsoid/works/blob/main/note/note5.md)

6-7. g2 악성세포, 항종양 group 조성 확인 [#](https://github.com/younsoid/works/blob/main/note/note5.md)
 
6-8. g9 grouping

6-9. 보조 오믹스의 설명력

6-10. 29 signature의 보조 오믹스 채택

6-11 grouping 별 보조 오믹스 채택

### 7. 모델과 학습

7-1. 모델 설계 [#](https://github.com/younsoid/works/blob/main/note/note17.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note18.md)

7-2. 학습 설계 [#](https://github.com/younsoid/works/blob/main/note/note17.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note18.md)

7-3 기준선: 슬라이드 평균 특징 + 릿지 [#](https://github.com/younsoid/works/blob/main/note/note19.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note20.md)
 
7-4. 실행 [#](https://github.com/younsoid/works/blob/main/note/note19.md)

### 8. Attention과 g7 grouping

8-1. attention 확인 [#](https://github.com/younsoid/works/blob/main/note/note8.md)

8-2. attention similarity matrix의 일관성 [#](https://github.com/younsoid/works/blob/main/note/note8.md)

8-3. g7과 g6의 유사성 [#](https://github.com/younsoid/works/blob/main/note/note8.md)

8-4. Attention similarity와 RNA coexpression [#](https://github.com/younsoid/works/blob/main/note/note10.md)

### 9. 성능 평가

9-1. 성능 지표 [#](https://github.com/younsoid/works/blob/main/note/note21.md)

9-2. 판정 [#](https://github.com/younsoid/works/blob/main/note/note21.md)

### cf

cf) 모델 기본 성능 [#](https://github.com/younsoid/works/blob/main/note/note6.md) / grouping 이득 확인 (luad) [#](https://github.com/younsoid/works/blob/main/note/note6.md) / grouping 이득 확인 (lusc) [#](https://github.com/younsoid/works/blob/main/note/note6.md)

cf2) 연구 배경 [#](https://github.com/younsoid/works/blob/main/note/note11.md)

cf3) attention head 그룹화 [#](https://github.com/younsoid/works/blob/main/note/note15.md)
