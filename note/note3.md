# 6. Grouping과 보조오믹스

### 6-1. grouping과 비교 설계

먼저 정의한 grouping은 g2(Bagaev 4 group), g6(rna coexpression k=4), g9(rna pc1,2,3), g7(co-attention)이다.

검증용 grouping은 g0은 signature별 29 heads, g1은 1head, gr은 g2와 k가 같은 group이다.

그리고 g8은 4 head를 두고 모델이 grouping을 스스로하도록한 설계인데 아직 결과를 내지 않았다. 

즉 domain based grouping인 g2가 있고 data based grouping이 g6, g9, g7, g8이 있다. 

### 6-2. g6 grouping 확인

29 signature 끼리의 spearman correlation을 사용해서 dendrogram을 그리고 silluette score가 가장 큰 k에서 잘랐다. lusc, luad 모두에서 k=2, size [28,1]이 나왔고 둘다 1은 proliferation이었다.

silluette score는 lusc 0.55, luad 0.38~~0.42 정도로 lusc는 강한 구조, luad는 약한 구조이다.

여기서 볼수잇는건 29 signature를 1, 28로 나눈 면역 흐름은 "미세환경 세포가 얼마나 많은가"라는 축이고 그 축을 기준으로 proliferation과 나머지가 반대 경향성을 보인것이다. 그리고 lusc는 28 signature들이 강하게 일관된 경향을 보인것이고 luad는 상대적으로 낮으니까 그 아래에 두번째 구조가 있을것으로 예상할수있다. 그래서 k=3, 4로 잘라보면 k=4와 k=2가 score가 유사하므로 두번째 구조의 존재를 유추 가능하다. 결론적으로는 어쨌든 28 vs 1 이 뚜렷하므로 k=2로 묶으면 모델이 그 축만 잘 잡아도 대부분을 맞혀버리기 때문에 묶는 방식에서 오는 성능 개선은 적을 것으로 생각할수있다. 

[28,1]에서 떨어져나온게 proliferation이었는데, 이유를 생각해보면 proliferation은 종양세포가 많을수록 높아지는 종양 purity 신호이다. proliferation signature는 세포 분열 유전자로 구성돼있는데 폐암 조직에서 활발하게 분열하는 세포는 주로 암세포이기 때문이다. 즉 암세포가 많은 조직일수록 proliferation signature가 높을것으로 생각할수있다.

이를 실제로 확인해보기위해서 ABSOLUTE purity와 proliferation 사이 correlation을 확인해본 결과 lusc에서 ρ=+0.41로 높게 나왔다. 그리고 Thorsson 기질 비율 사이 correlation 을 확인해봤을때는 ρ=-0.42로 낮게 나왔다. 즉 lusc proliferation은 암세포가 많으면(purity) 높아지고 기질 비율이 많으면 낮아진다. 순도와 기질 비율은 RNA 가 아닌 타 오믹스로 얻은 자료이므로 실제 조직 구성을 반영한 결과로 생각할수있다. 

반면 luad 에서는 각각 +0.04, -0.04로 무관하게 나왔다. 즉 proliferation과 purity는 상관관계가 없었다. 이는 proliferation 이 암세포의 정량보다 암세포가 얼마나 빨리 분열하는지를 반영한 값이라는 뜻이다. (해석: 즉 천천히 분열하는 luad와 빨리 분열하는 luad의 차이가 purity 차이보다 크다는 것이다.)

그리고 실제로 correlation distance를 보면 luad는 타 signature 과의 평균 ρ(상관)이 -0.02, lusc는 -0.26이다. 즉 luad와 lusc 모두 proliferation이 떨어져나왔는데 각기 다른 이유인것이다. lusc는 음의 상관이어서 멀고, luad는 무상관이어서 나머지 28 signature와 멀었다. 

따라서 lusc와 luad에서 proliferation이 떨어져나왔다는 사실은 같지만, 모델 입장에서는 두 경우가 다르다. lusc는 proliferation이 큰 축의 반대 방향이어서 미세환경이 얼마나 많은지를 읽는 모델은 부호를 뒤집어서 proliferation을 맞힐 수 있다. 즉 타 미세환경 signature와 같은 head에 묶여도 같은 곳에 attention을 둘수 있어서 부정적 전이가 낮을것으로 생각할수있다. laud는 proliferation이 중요한 큰 축과 직교한다 즉 무상관이다. 미세환경의 양을 잘 읽더라도 proliferation을 맞히는것과는 상관이 없다. 그래서 luad는 부정적 전이 위험이 크고 luad proliferation을 맞히려면 종양 세포 자체(핵모양, 분열상 등등)를 볼수있는 다른 근거가 필요할것이다.
