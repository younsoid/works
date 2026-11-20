### 6-5. k=4 g6 grouping (2)

6-4에서 말했듯이 grouping 결과는 luad, lusc 각각 [18,9,1,1] [17,9,2,1] 였고 구성은 면역/기질/neutrophil/proliferation 으로 구성돼있었다.

group 내 안정성 수치인 fold 간 ARI는 0.97/0.87로 안정적이었고 n수가 큰 면역/기질 group의 group간 ρ는 0.26/0.36으로 낮은 편, group 내 ρ는 0.54–0.58/0.60–0.63로 높은 편이었다. 

neutrophil group과 면역/기질 group 간 ρ는 각각 luad 0.05/0.25, lusc 0.20/0.43이었다. 

proliferation과 면역/기질 group 간 ρ는 각각 luad 0.00/−0.05, lusc−0.21/−0.36이었다. 

이걸로 알수있는점은 28 signature의 큰 축은 면역과 기질이었고 luad에서 group의 경향성이 조금더 강했다. 그래서 g2와 달리 면역/기질을 별도의 head로 나누는 설계의 학습 이득이 laud에서 조금 클것으로 생각된다. 

그리고 Bagaev의 4 subtype에서 각 group이 어느 subtype을 얼마나 따라가는지를 확인했을때, 면역 group은 IE, IE/F에서 높았고 기질 group은 F, IE/F에서 높았다. 

neutrophil group은 F에서 가장 높았고 proliferation은 D, IE에서 높고 IE/F, F에서 낮았다.

즉 Bagaev subtype을 나눈게 면역/섬유화 축이었는데 signature coexpression 측면에서도 그 축의 분류를 어느정도 따랐다. 특히 proliferation이 섬유화 관련 subtype에서 매우 낮았는데 (IE/F랑 F) 그래서 proliferation에 별도의 head를 할당하는 설계가 D 뿐만 아니라 F 계열 구분에도 사용할수있을것으로 보인다. 

### 6-6. 각 grouping의 결속 측정

한 head에 묶인 signature들은 하나의 attention으로 맞히게 된다. RNA가 공발현될수록 하나의 부위를 볼것이라고는 단정하기 어렵지만, RNA가 공발현되지 않는 즉 결속력이 낮은 group이라면 하나의 축이면서 경향성이 완전히 반대인 경우라면 하나의 생물학적 축을 따를수도 있지만 그렇지 않은 경우에는 별도의 생물학적 축을 따름에 따라 근거가 다른 부분에 있을 가능성이 크다. 즉 부정적 전이가 있을 가능성이 있다. 그래서 grouping 별로 각 group의 이질성을 수치화하여 미리 그 결속력을 확인하였다. 

group 내 ρ는 일단 무작위 그룹이 0.35–0.41/0.39–0.46으로 이미 좀 높다. 그리고 g6(k=4)가 0.58/0.62, g2가 0.53/0.58, g9가 0.49/0.46 순으로 결속력이 좋았다. 

무작위 그룹도 높은 이유는 29 signature가 미세환경 특성을 반영한 gene set으로써 이미 면역이라는 같은 축을 공유하는 데이터셋이기 때문이다. 그래서 gap이 GR(무작위 group)보다 큰지를 봐야하는데 그 기준으로 봤을때는 g6,2,9가 모두 GR보다는 결속력이 강하다. 

그리고 grouping 각각을 봤을때는 g2(n=4)[0.63, 0.44, 0.56, 0.20], g6(n=4)[0.59, 0.54, –, –], g9(n=4)[0.44, 0.43, 0.55, 0.46]  /  g2 [0.64, 0.52, 0.64, −0.23], g6 [0.63, 0.59, 0.65, –], g9 (n=2) [0.46, 0.44]로 g2의 악성세포 group이 결속력 두 subtype 모두에서 0.20 -0.23으로 결속력이 가장 낮았다. 친종양 group은 lusc에서는 0.52, luad에서는 0.44로 낮은 편이다. 그리고 악성세포 group이 위 결과대로 proliferation이 섞인 group이다. 

### 6-7. g2 악성세포, 항종양 group 조성 확인

g2의 악성세포 group과 항종양 group에 속한 signature들을 확인하고 g6(k=4)에서는 어떻게 grouping 되었는지, 어떤게 달라졌는지를 확인해본다.

우선 g2의 4 group인 항종양/혈관신생및섬유화/친종양/악성세포 group에서 group 내 ρ를 보면 항종양은 0.61/0.63, 혈관신생및섬유화는 0.59/0.65, 친종양은 0.41/0.52 (친종양-항종양이 0.42/0.48이므로 비슷함), 악성세포는 0.19/-0.22였다. 

즉 항종양, 혈관신생및섬유화 group은 signature 가 coexpression 되었고 친종양은 약하고, 악성세포는 거의 무상관이었다. 생물학적으로 보면 T cell이 활성화되면 checkpoint molecule과 T reg도 같이 올라가므로 맞는 결과이다. 

그러므로 g2에서 항종양-친종양 head가 같은 림프구 enriched 부위를 볼수도 있다. (이건 사실 histotme에서도 나옴)

그리고 g2의 4 group이 g6에서는 어떻게 분포한지를 보면, 항종양 11 sig들은 모두 면역 group, 혈관신생및섬유화 5 sig들은 모두 기질 group, 친종양 11 sig들은 면역/기질/호중구에 7/3/1 6/3/2로 흩어진다. checkpoint molecule, Treg, Th1, Th2, macrophage가 면역 group으로 들어가고 친종양 cytokine, mdsc immigration, 과립구 immigration은 기질 group으로 들어갔다. 그리고 악성세포 2 sig 들은 각각 기질(EMT), proliferation으로 들어갔다. 

그러므로 지금 설계로 보면 친종양 head가 g6 group 측면에서는 면역, 기질, 호중구 3 group을 예측하게된다. 그리고 악성세포 head는 기질, proliferation을 함께 예측한다. 이 둘은 반대 성향임을 확인한 두 sig이다. 

즉 Bagaev의 기능 분류 4 group은 29 sig 의 co expression과 잘 맞는것도 있고 아닌것도 있으며 아닌 경우에 g2와 g6(k=4)의 성능 및 attention 해석이 중요해보인다. 
