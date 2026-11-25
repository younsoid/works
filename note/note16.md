### 1-6. 보조 오믹스별 정보량

보조오믹스 후보인 methylation 과 rppa가 29 signature를 각각 얼마나 설명하는지 미리 확인하기 위해서, 두 데이터를 각각 normalization 하고 pca를 통해 pc 20개로 차원 축소하였다. 그리고 20 pc feature로 29 signature score를 lidge regression으로 예측하여 R2 값을 확인하였다. regression은 5 fold 교차검증을 seed 3로 반복한 후 3 seed의 평균 R2를 계산했다.

raw data 크기는 methylation 과 rppa 각각 400,772 (or 397,085) x 398 (or 338), 455 x 311(or 289) 였는데 rppa는 그대로 pca에 넣었고 methylation은 variation 상위 5000개 탐침을 사용해서 pca 하였다. 

그리고 비교를 위해 RNA 도 똑같이 pca 후 lidge regression 해서 R2를 계산했는데, mRNA도 약 2만x452(or 453)으로 커서 variation 상위 5000개 gene만 사용해서 pca 하였다. 

normalization의 경우 methylation은 raw beta 값을 log2(β / (1 − β))한 M 값으로 변환하였고 RNA는 기존에 log2(TPM + 1)을 한걸 그대로 사용했다. rppa는 추가 정규화하지 않았다. 그리고 methylation rppa rna 각각 5000, 455, 5000 개 중 결측이 10% 넘는 feature를 제외하고 남은 결측은 평균값으로 채운 뒤 z score 변환 하였다. 

예측 결과 mRNA는 luad/lusc에서 0.747 / 0.755, rppa는 0.363 / 0.339, methylation은 0.352 / 0.345가 나왔다. R2는 해당 feature가 데이터의 몇%를 설명하는가인데, rppa와 methylation 모두 35% 정도를 설명함을 확인하였다. 

signature별로 보조오믹스로 무엇이 선택될지도 간략하게 확인했는데, luad는 rppa 20, methylation 9 / lusc는 rppa 10, methylation 19로 나름 골고루 분포하는것을 확인할수있었다. 
