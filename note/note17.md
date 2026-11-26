# 07 모델과 학습

### 7-1 모델 설계

연구 목적은 wsi를 받아서 29 signature를 예측하는 모델을 만드는데, 이때 attention head를 하나로 두고 29 signature를 예측하게 하지 말고 특정 방식으로 grouping하여 head를 할당하면 예측 성능과 attention 부위가 어떻게 바뀌는지를 확인하는 것이다.

baseline 모델로는 attention MIL을 사용하였다. 이 모델은 feature extractor와 ?? 로 나뉘고 ?? 는 다시 ?? 와 ?? 로 구성된다. (각각 모델이 어디를 볼지 / 그 부위를 보고 어떤 예측을 낼지 를 담당)

feature extractor는 EXAONEPath를 사용해서 slide의 각 타일을 (768,) 벡터로 변환하여 feature extract 하였다. 

그리고 ?? 에서 (768,) 벡터를 (512, ) 로 사영하였다. (ReLU, dropout 0.25)

첫번째 설계는 head 공유이며 논문 ?? 의 방식을 따랐다. 

그리고 ??에서는 group 개수만큼의 gated attention head (256차원)을 하나씩 뒀다. head는 그 group의 타일 weight를 만들고(slide 안의 합이 1이 되게) 합쳐서 slide 벡터를 만든다. 

그리고 ?? 에서는 signature마다 작은 회귀 MLP(512 → 128 → 1)를 두고 그 siganture MLP는 속한 group의 슬라이드 벡터를 받는다. (자신의 slide 벡터 아님)

그리고 ??에서는 손실을 표준화한 29개 시그니처의 평균 제곱 오차(MSE)로 본다. 

모델 구조는 이렇고, grouping의 경우 g0은 29개, g2,g6,g7은 4개 head를 뒀다. 

그리고 두번째는 보조 정답 설계이며 논문 ??의 방식을 따랐다. 구조는 head마다 보조 정답을 맞히는 branch를 붙이고 손실에 λ 0.5로 더하는 방식을 사용했다. 

위 구조를 가지고 모든 설계를 학습하였다 (8 design: g0, g2, g6, g7, g0+보조오믹스, g2+보조오믹스, g6+보조오믹스, g7+보조오믹스)

### 7-2 학습 설계

학습 설정은 AdamW, cosine learning rate 사용하였고 g0에 대해 돌려 lr, wd를 측정했다. epoch은 최대 50 epoch에 early stopping 을 10으로 했다. (val 손실이 10 epoch 동안 나아지지 않으면 멈추고 best 모델을 씀)

3 seed 5 fold로 학습하는데 lr, wd 측정은 1 seed 1 fold만 수행해서 (총 학습 수 60회) val 손실을 보고 결정하였다.

lr, wd 측정 결과 lr 1e-4, wd 1e-4가 나와서 이 값을 최종 학습에 사용했다. (총 학습 수 ??회)

그리고 추가로 고정 epoch 학습도 수행했는데, g0 학습때 learning curve 확인 결과 epoch 2-3에서 early stopping 된것을 확인했다. (학습 곡선에서 val 손실이 epoch 2-3에서 최저, 그 뒤로는 train 손실만 계속 감소함 즉 과적합 양상) 그래서 g0와 다른 grouping 학습을 비교할때 만약 특정 grouping 학습의 epoch이 10 이상으로 높은 경우가 있으면, attention이 특정 타일에 과하게 몰린다. (실제로 g0 실행들의 attention 집중도와 정지 epoch를 확인해보니 늦게 끝날수록 집중도가 높았다 ρ 0.92–0.97 즉 오래 학습할수록 모델이 확신을 갖고 특정 타일만 봐서 attention이 특정 타일에 몰리게 된다) 

그래서 특정 grouping 학습의 attention 세기가 g0보다 높다 해도 그 grouping에 의한 결과가 아니라 g2가 더 늦은 epoch에서 멈춘 것에 따른 결과일수있다.

그래서 고정 epoch을 학습을 수행했다. 학습 길이를 모든 실행에서 맞춰서 민감도 분석을 하는것이다. 5로 고른 이유는 g0 학습의 val 곡선을 봐서 epoch마다 최초 30회의 평균 val 손실을 구하고, 가장 낮은 epoch를 찾아 "최저점 + 표준오차 안의 가장 늦은 epoch"를 골랐을때 5였다. 즉 epoch 0-4였다. 

가장 늦은 epoch를 고르는 이유는 성능이 최저점과 같은 수준이면서 attention이 충분히 안정되는 시점이어서이다. 학습 곡선(R1)에서 epoch 4는 모든 설계의 val 바닥 구간 안에 있었으니, 이 선택은 G0뿐 아니라 다른 설계에도 맞았다. 

최종 학습 결과 learning curve를 보면 모든 실행이 epoch 2-3정도로 배웠다. 근데 10 이상도 있었다. epoch 0 대비 val 손실 감소는 0.067-0.081로 유사했다. 
