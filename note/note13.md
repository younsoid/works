### 1-2. MFP 29 signature 재현

예측 대상인 Bagaev 29 signature는 원 데이터에서 893명에 대해서 계산되어있다. 

Bagaev 29 signature의 계산 방식에 따라 폐암 데이터 905명에 대해서 계산하고, 원 논문과의 재현성을 Spearman correlation 값으로 확인했다.

재현성 확인 결과 Spearman correlation 0.991로 Bagaev 29 signature가 905명에서 잘 재현되었음을 확인할수있었다. 

### 1-3. MFP subtype 재현

Bagaev 연구에서는 29 signature를 기반으로 샘플을 IE, IE/F, F, D 4 group으로 분류하였다. 29 signature를 feature로 하여 knn(k=35)를 학습하였고 905명에 대해서 재현한 결과 일치율 84.9%로 높았다. 혼동행렬을 확인했을때 Fiberation 관련 group인 IE/F, F group에서 다소 일치도가 낮은 경향이 있었다. 이에 대해서는 2가지로 해석이 가능한데 첫번째는 원 논문에서 IE, IE/F, F, D 가 해당 순서로 Immune rich -> deplete 축에 해당한다고 언급하고 있어서, 양 극단 group에 비해 연속 축에 해당하는 IE/F, F의 재현율이 낮았을 수 있다. 두번째는 해당 grouping은 범암종 grouping이고 905명은 폐암 데이터셋임에 따라 Fiberation이 폐암에서 타 암종과 조금 특이성을 보이는 축일 수 있다. 

### 1-4. MFP score 보조 데이터

1-4-1. 미세환경 관련 외부데이터

29 mfp score 외에도 미세환경 외부데이터를 3개 수집했다. Yoshihara 연구의 ESTIMATE, Becht 연구의 MCP counter, Thorsson 연구에서 재계산한 CIBERSORT 데이터이다. 각각 2개, 10개, 22개 항목이다.

여기서 ESTIMATE와 MCP counter는 RNA로 계산한 값이고 CIBERSORT는 RNA+Methylation 이다.

1-4-2. 종양 관련 외부데이터

ABSOLUTE의 tumor purity와 ploidy 데이터 외에도 종양 관련 외부데이터를 2개 수집했다. Taylor 연구의 aneuploidy score, Aran 연구의tumor purity이다. (ESTIMATE, ABSOLUTE, LUMP, IHC, CPE) 앞의 4개 각각은 RNA, ploidy, methylation, 병리의 판독으로 계산한 값이고 CPE는 4개 항목의 합이다. 
