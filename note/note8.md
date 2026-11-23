# 8. Attention과 g7 grouping

### 8-1. attention 확인

g0은 signature 마다 head를 할당하는 방식이고, g6, g9는 rna coexpression과 pc123으로 head를 grouping했다. 

g0 방식으로 일단 모델을 한번 돌려서 모델의 attention이 비슷한 signature끼리 grouping하는게 g7이므로 g0 attention으로 attention similarity matrix를 만들고 g6처럼 hierachical clustering해서 g7을 만들었다. 

g0 attention 결과 similarity matrix 내 값은 3 seed 동안 평균 laud [0.322, 0.479, 0.550, 0.586, 0.596] lusc [0.418, 0.436, 0.363, 0.393, 0.376] 로 나왔다. (5 fold)

그리고 silluette score로 최적 k는 10 fold 모두 k=2가 나왔다. luad 5 fold는 [28,1] 4번, [25,4] 4번이었다. lusc 5 fold는 [28,1]이 2번, [20,9], [18,11], [24,5]가 나왔다. 

k=4로도 돌려보았는데 luad lusc 모두 큰 group 1개와 작은 group 3개였고 luad는 Neutrophil 5(모두 단독), Endothelium 5(단독 4), B_cells 3, Granulocyte_traffic 2, CAF 1번 밖으로 나왔다. lusc는 매번 달랐는데 기질 계열(CAF, Matrix, Matrix_remodeling, Angiogenesis)·Endothelium·Proliferation이 3번 나왔다. 

즉 laud에서는 Neutrophil, Endothelium, Bcells가 fold 바뀌어도 따로 나왔다. lusc는 fold마다 달랐다. 

### 8-2. attention similarity matrix의 일관성

attention similarity matrix는 attention 학습때마다 생성되므로 seed에 따라 달라질수있다. 

그러므로 seed마다 similarity matrix를 만들었다. (3 seed x 2개 matrix) 각 29x29 matrix는 29x28/2 = 406개 값을 가지니까 seed 0,1,2에 대해서 fold 5개에 대해 Spearman correlation을 구했다 (3x2/2) x 5 = 15번. 

확인 결과 laud는 평균 0.653, lusc는 평균 0.418이 나왔다. 즉 luad는 seed가 달라도 형태 유사도 구조가 대체로 재현되었다. lusc는 절반 이하만 재현되었다. 

### 8-3. g7과 g6의 유사성

g6은 rna coexpression으로 grouping 했고, g7은 attention similarity로 grouping을 했다. 

여기서 g6에서는 grouping 되었는데 g7에서는 grouping되지 않은 경우는 coexpression 되었지만 모델의 attention이 다른 경우이다. 

이를 확인하기 위해서 먼저 5개 fold 마다 g6, g7 사이 ARI를 계산했다. ARI는 grouping이 완전히 같으면 1, 아무 관계가 없으면 0이다. 

그리고 g6-random을 사용해서  g7과 ARI를 측정한 random-ARI 분포를 만들어서 ARI와 비교, random의 상위 5%면 유의한 ARI로 보고 g6과 g7에서 같은 grouping이 됐는지 다른 grouping이 됐는지를 확인했다. 

먼저 g6에서는 grouping 되었지만 g7에서는 떨어진 pair는 406개 중 luad 26, lusc 17 pair였다.

luad는 26개 중 17 pair가 B cell - {T cell 등 면역 signature} 였고 8 pair가 Endothelium - {기질 signature} 였다.

lusc는 17개 중 6개가 Endothelium - {기질 signature} 였고 Treg 5개, Macrophages 5개, B_cells 3개 였다. 

그리고 g6에서는 떨어져있었는데 g7에서 grouping 된 pair는 luad 161, lusc 87 pair였다. 이중 5 fold 모두에서 grouping 된 pair는 laud 125, lusc 10개였는데 luad 125 중 102가 면역-기질 group 간이었고 Proliferation rate-면역 이 17, Proliferation rate-기질 이 6이었다. 호중구는 fold별로 경향이 달랐다. 

즉 g7 group은 g6 group과 크게 다르다(ARI 0.2). 그리고 g6 떨어짐 -> g7 grouped 가 많은것은 g7 group에서 하나의 group이 대부분의 n 수를 차지하고 있어서일수있다. 

반대로 g6 grouped -> g7 떨어짐은 luad에서 B cell, Endothelium에 몰렸고 lusc에서 Endothelium, Treg·대식세포·B 세포에 몰렸다. 이 경우는 RNA로는 같은 group이어도 모델이 그 signature를 맞힐때 쓰는 조직 근거가 다를 수 있는 후보다. 

또한 g6 모델의 attention(사실상 g7)과 g7 모델의 attention 사이 차이를 보면 rna grouping의 효과와 attention grouping의 효과를 나눠서 볼수있다.
