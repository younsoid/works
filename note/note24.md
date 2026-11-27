### 6-0. Fold design

이 연구는 29 signature를 어떻게 묶어서 head를 공유시키는지가 성능과 attention을 바꾸는지를 본다.

여기서 확인할점 2가지가 있는데 첫번째로 train과 test에서 같은 기관이 있으면 모델이 생물학적 특성보다 기관 특성을 예측 판단에 사용하게된다. 그래서 성능이 부풀려진다. 그래서 기관 단위로 fold를 나누었다. 

두번째로 head 공유의 효과가 head를 어떻게 공유시키는지에 따른 것인지 단순히 head를 공유했다는 사실 때문인지를 확인해야 한다. 그래서 G2와 묶음 크기가 같고 (11, 11, 5, 2) 내용이 무작위인 GR 20 set를 만들어서 같이 비교하였다. 

먼저 전체 샘플은 laud 442명 (33개 기관), lusc 453명 (tss 35개 기관)이었다. fold 마다 test 83-95명이며 test와 train, train과 val 사이 기관 겹침이 0이 되도록 train fold를 만들었다. 또한 학습 fold 안에서도 기관 단위로 80/20을 나눠 조기 종료용 val을 만들었다.

샘플을 MFP subtype으로 층화한 결과 fold 마다 subtype이 고르게 분포함도 확인하였다.

### 6-1. Head design

이 연구는 29 signature를 어떻게 묶어서 head를 공유시키는지가 성능과 attention을 바꾸는지를 본다.

그래서 Head를 어떻게 묶을지, Head design을 다음과 같이 해줬다.
- H0: 안묶음 / 29 heads (1)
- H1: 전부 하나로 묶음 / 1 head (29)
- Hbag: Bagaev 연구의 생물학적 의미로 묶음 / 4 heads (11, 11, 5, 2)
- Hbag_random: Hbag 같은 크기, 무작위로 묶음 / 4 heads (11, 11, 5, 2)
- Hrna: RNA coexpression으로 묶음, 데이터 기반 / 4 heads (fold 마다 다름)
- Hatt: H0의 attention similarity로 묶음, 데이터 기반 / 4 heads (fold 마다 다름)

그리고 design 간 비교는 아래와 같이 계획하였다.
- H0 vs H1: 묶음 자체의 효과
- Hbag vs Hbag_random: 묶은 내용의 효과
- Hrna vs Hattn: RNA 구조로 묶는것과 Attention 구조로 묶는것의 차이 (= 정답 쪽(RNA) 구조로 묶는 것과 입력 쪽(형태) 구조로 묶는 것의 차이)

Head design별 학습 설계는 모두 동일하다. 타일별로 feature extraction -> 공통 투영 -> head design 별 gated attention 하나 -> signature마다 자신의 mlp 출력. 즉 design이 바꾸는 것은 어떤 signature들이 같은 attention map으로 슬라이드를 요약하는가 이다.
