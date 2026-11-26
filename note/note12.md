### 1-1. 코호트 구성

#### 1) 오믹스 현황 파악

dx slide, rna가 모두 있으면서 do not use가 아닌 샘플은 905명(452/453)이다. 

오믹스 데이터는 변이(MAF), miRNA, methylation, rppa가 있다. 

변이는 444/440, miRNA는 446/440, methylation은 398/338, rppa는 331/289명에 존재한다. 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/a4dff25f-ecfc-4edb-b520-caf6d9a7706b" />


#### 2) 오믹스 전처리

변이는 비동의 변이만 남겼다. (221,514/296,382, 74.7%)

miRNA는 필터링은 진행하지 않고 log2(RPM+1) processing 하였다. 

methylation은 cg filtering을 해서 ch, rs를 제거하고, Zhou 연구의 450K hg38 mask 데이터를 사용해서 hg38에 제대로 매핑되는 값만 남겼다. 그리고 성별 차이에 따른 methylation을 제외하기 위해 X Y 염색체 filtering하여 486,427 중 442,243개를 남겼다. 

그리고 결측이 20% 이상인 경우를 filtering하고, 20% 미만인 경우 평균값으로 채웠다. 결과적으로 486,427중 400,772/397,085개가 남았다. (이 작업은 laud/lusc 따로 진행)

<img width="1200" alt="fig01_omics_qc_nozero" src="https://github.com/user-attachments/assets/9950e578-83fa-4879-9b0d-c1f056ee94e4" />

#### 3) 임상 및 외부데이터

GDC PanCanAtlas에서 TCGA-CDR의 임상 정보를 가져왔다. 나이, 성별, 인종, 병기, 조직형, 생존(OS/DSS/PFI), 흡연 7개 데이터이다. 

그리고 외부데이터는 PanCanAtlas의 ABSOLUTE, Thorsson 연구의 면역 table을 가져왔다. ABSOLUTE는 tumor purity와 ploidy 데이터이다. Thorsson 연구의 면역 table은 면역 아형,  백혈구 비율, 기질 비율, TIL 영역 비율, 변이율, TCGA 아형 6개 데이터이다. 백혈구 비율은 methylation 기반으로 계산한 데이터이고 기질 비율은 ABSOLUTE 순도 + 백혈구 비율로 계산한 데이터이다. TIL 영역 비율은 Saltz 연구에서 WSI로부터 계산한 값이다.

905명 중 나이, 성별, 인종, 병기, 조직형, 생존(OS/DSS/PFI), 흡연은 319-452/348-453, ABSOLUTE는 440/449, Thorsson 연구의 면역 table은 195–450/157–453명에서 존재했다. 

#### 4) 주요 유전자 변이와 비동의 변이 수

폐암 gene 6개 TP53, KRAS, EGFR, STK11, KEAP1, NFE2L2에 대해서 환자별 비동의 변이 수를 확인했다. 분석은 maf 변이 데이터가 존재하는 444/440명에 대해서 수행했다. 

결과를 보면 luad는 kras, egfr, stk11이 높고 lusc는 tp53, nfe2l2가 높다. stk11, keap1은 luad에서 높은데 cold tumor와 연관된 변이다.

