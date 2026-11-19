# 1. 데이터 준비

### 1-1. 코호트 구성

연구의 목적은 h&e slide로 rna msi 점수를 예측하는 것이다. 

그래서 데이터로는 dx slide, rna가 모두 존재하는 대상자를 사용했다. 

luad subtype은 452명, lusc subtype은 453명을 사용해서 총 905명이 예측 대상으로 사용되었다.

905명이 가진 데이터 종류는 변이(maf), miRNA, 복제수 aneuploidy score, methylation, rppa, thorsson table, survival data, age, 병기. 

복제수 aneuploidy score는 Taylor 연구에서 매긴 숫자이다. Thorsson table은 면역 아형 C1~C6, 백혈구 비율, 기질비율, TIL 영역 비율, 비동의변이율, CIBERSORT의 22개 면역세포 비율 등의 정보다. 

생존 자료는 생존 유무랑 시간 데이터고 병기는 진단 시점의 암이 몇기인지 1~4기로 나눈 값이다. 

### 1-2. MFP 29 signature 재현

연구의 목적은 Bagaev 연구의 29 msi score를 예측하는 것이다. 

905명의 rna 데이터로 Bagaev 연구의 29 msi score를 계산해서 905명x29 msi score table을 만들었다. 

그리고 Bagaev 연구에서 사용한 29 msi score와 수치를 비교해봤는데, Bagaev와 공통인 893명만 가지고 계산했고 Spearman ρ가 0.991이 나와서 거의 일치함을 확인했다. 그래서 893명만이 아니라 905명 data를 사용할수있게 됐고 정당성도 검증됐다.

### 1-3. MFP subtype 재현

Bagaev 연구는 29 score를 기반으로 데이터를 4 subtype으로 나눴다.  

(29 signature를 나눈게 아니라 환자들을 나눈것임)

각각 IE, IE/F, F, D로 immune rich, immune rich+섬유화, 섬유화, immune depleted 이다. 

histotme 연구에서는 4 subtype을 따라 attention head를 묶어서 학습했었고 성능이 아주 조금 올랐었다. 

나눈 방법은 23개 암종 7031명을 가지고 29 signature을 feature로 한 knn을 학습시켜 분류했고 폐암에서의 분류 결과는 893명에 대해 각각 -명/-명/-명/-명이었다. 

그래서 우리도 이를 재현하기 위해 23개 암종 7031명을 가지고 29 signature을 feature로 한 knn을 학습시켰다. 이때 폐암은 빼고 학습시켜서 22개 암종 _명을 가지고 4분류 knn 을 학습하고 이를 우리 905명의 4 subtype 분류에 사용하였다. 

분류 결과 laud 452명에서 각각 128/63/93/168로 분류되었고 lusc 453명에서 104/63/89/197로 분류되었다. 

이를 893명만 꺼내서 Bagaev 연구와 비교해보니 폐암을 쓰지 않고도 Cohen κ 일치도 0.791로 분류가 일치했다. subtype 별 분류 재현율은 90.1/72.0/78.7/91.4%로 양극단 특성인 immune rich와 immune depleted는 재현율이 높았고 immune rich+섬유화/섬유화가 재현율이 좀 낮았다. 

여기서 알수있는건 4 subtype이 뚜렷한 범주라기보다는 면역->섬유화->면역결핍 의 연속 축이있고 그걸 구간화한것이라는 걸 알수있다. 즉 아형 경계 자체가 흐리다는 정답의 성질이 있다.  

### 1-4. msi score 보조 데이터

Bagaev의 29 msi score만 쓰면 msi 자체의 특성보다는 그 방법론의 계산방식과 gene set에 치우칠수 있어서 MFP score 외에 3개 검증용 점수를 구했다. 

Yoshihara 연구의 ESTIMATE, Becht 연구의 MCP-counter, Thorsson 표의 22 immune cell rate에 wbc rate를 곱한 CIBERSORT 절댓값을 계산해서 추후 검증용으로 사용한다. 
