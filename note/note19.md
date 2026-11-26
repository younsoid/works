### 7-3 기준선: 슬라이드 평균 특징 + 릿지

attention 모델이 단순한 모델보다 성능이 나은지를 확인해서 성능이 비슷하다면 attention 모델의 의의를 성능 개선 vs 해석(어디를 보는가)으로 결정한다.

타일 feature들의 (768,) 벡터를 평균해서 슬라이드 벡터를 만들고 ridge regression 을 수행했다. fold 분할은 같이 수행했고 g0 seed0과 R2 성능을 비교했다.

확인 결과 g0 seed0  0.353/0.315, ridge 0.357/0.327로 비슷했다. signature별로 보면 CAF, MHC1은 ridge와 유사하거나 ridge가 높기도 했지만 neutrophil, endothelium은 확연히 낮았다. 

즉 전체 signature 로 보면 attention 모델의 가치는 성능이 아니라 어느 부위를 보는지 알려주는 것에 있음을 알수있다. neutrophil, endothelium의 경우는 attention 처럼 특정 타일을 골라 보는 방식이 성능, 해석 둘다 측면에서 필요할 수 있다. 

### 7-4 실행

모든 설계 × 조직형 × fold × seed에 대해 학습을 실행하였다. 

실행은 g0, g2, g6, g7이 30 / g0, g2, g6, g7 + 보조오믹스가 120 / gr(무작위 그룹) 200 / g1 30 / learning rate 측정 (g0, g2, g6, g7 x 25·50·75%) 120 이렇게 수행하였다. 
