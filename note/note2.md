### 1-5. 보조 오믹스

rna 외의 오믹스는 메틸화, miRNA, RPPA, 복제수 aneuploidy score가 있었다. 

이 오믹스 데이터는 2부분에 쓰이는데 하나는 attention 학습의 보조정답이다. head마다 메틸화, RPPA, 복제수 aneuploidy score중 하나를 골라서 보조 오믹스로 채택하고, 그 보조 오믹스로 계산한 정답을 보조 정답으로 넣어주었다. 

두번째는 모델의 예측잔차 설명에 사용하는데, 모델이 "T cell activation"에 대해서 rna로 0.9를 예측했고 정답이 1.5라고 하면 0.6이 잔차이다. 이 잔차는 모델이 잘 학습되었다는 전제 하에 "사진으로 확인할수없는 데이터"이다. 

"T cell activation"의 수백명의 잔차를 모으면 그 signature에서 "사진이 놓친 부분"을 확인할수있다. 이 잔차가 그냥 잡음인지, 아니면 다른 오믹스로는 잡히는 생물학적 정보인지를 확인하기 위해서 그 잔차들을 메틸화, RPPA, 복제수 aneuploidy score 로 각각 회귀해서 각 오믹스가 잔차를 얼마나 맞히는지를 보는 분석을 할것이다. 

그 오믹스가 원래 맞힌 점수보다 잔차 맞힌 점수가 더 높으면 메틸화에는 사진에 없는 추가 정보가 있다고 볼수있을것이다. 

### 1-6. 보조 오믹스별 정보량

현재 attention head마다 rna가 아닌 오믹스 하나를 보조 오믹스로 채택해서 그 오믹스의 예측값도 같이 정답으로 넣어주는게 연구 설계인데, 그 오믹스가 MFP signature에 대한 설명력이 있는지, 그리고 오믹스마다 잘 설명하는 signature가 다른지를 확인해야한다. 설명력이 0이면 이상한걸 같이 예측하라고 보조로 넣어주는거니까 성능이 떨어질것이고 하나의 오믹스가 모든 signature에서 다른 오믹스보다 설명력이 높으면 다르게 넣어주는 설계가 무의미하기 때문이다. 

연구 전에 기대하는 바는 면역/기질 signature는 methylation이 보조 오믹스가 될거라고 생각했고(wbc, 기질 비율 등이 methylation으로도 추정되기 때문에) 증식 같은 tumor signature는 복제수 score나 RPPA가 보조 오믹스가 될거라고 기대한상태였다. 그리고 보조 정답이 도움이 될지 안될지에 대한 R2 기준은 우선 0.10으로 정했다. 보조 정답이 굳이 필요없고 rna 로 내는 성능이 최선일수도 있기 때문이다. 

그래서 먼저 오믹스마다 input feature를 만들고, 각 feature를 pc 20개로 줄인 후 29 signature를 릿지 회귀로 예측했다. 그리고 mRNA로 만든 feature/pc20개도 예측 성능을 뽑고, 29 signature 각각에 대해서 메틸화, RPPA, 복제수 aneuploidy score 중 R2가 가장 큰 층을 보조 오믹스로 채택하였다. 

분석 결과 29 signature 모두에서 보조 오믹스로 채택된 오믹스의 R2가 0.10을 넘어서 모든 signature에 대해 보조 오믹스가 채택됐다. 그리고 메틸화와 RPPA의 설명력이 대부분 비슷했고 복제수 aneuploidy score는 0에 가까웠다. 

signature마다 채택된 보조오믹스는 laud - rppa(20) methylation(9) 였고 lusc - rppa(10) methylation(19)였다.

그리고 보조오믹스의 R2가 높아서 해석이 기대되는 signature는 laud - Proliferation_rate(rppa 0.627), B cell(methylation 0.576), lusc - b cell (methylation 0.573) 정도였다. 해석이 어려워보이는 R2 낮은 signature는 luad - Neutrophil_signature(0.104), Angiogenesis(0.165) ,lusc - M1_signatures(0.183)이었다. 

+) R2가 높은걸 몇개 더 뽑아보면 

luad - Proliferation_rate(RPPA 0.627), B_cells(메틸화 0.576), Checkpoint_inhibition(RPPA 0.560), MHCII(RPPA 0.551), Coactivation_molecules(메틸화 0.550), T_cells(메틸화 0.482), Effector_cells(RPPA 0.480), NK_cells(RPPA 0.473), T_cell_traffic(RPPA 0.471)
lusc - B_cells(메틸화 0.573), Checkpoint_inhibition(RPPA 0.545), T_cells(메틸화 0.545), Effector_cells(RPPA 0.532), Coactivation_molecules(메틸화 0.511), Th2_signature(메틸화 0.509), MDSC_traffic(메틸화 0.506), NK_cells(RPPA 0.493), T_cell_traffic(RPPA 0.475)

인데 전체적으로 보조 오믹스가 잘 맞히는것은 림프구 관련이다. 

B_cells, T_cells, Coactivation_molecules는 두 subtype 모두 methylation, Effector_cells, NK_cells, T_cell_traffic, Checkpoint_inhibition은 모두 rppa가 채택되었다 특히 B_cells는 methylation과 RPPA의 차이가 0.21~0.27로 가장 커서 보조 오믹스 효과가 제일 크게 차이났다. Proliferation_rate는 methylation이 선택됐는데 luad에서는 R² 0.627로 가장 높았지만 lusc에서는 0.334(메틸화)로 낮다. 그래서 이 signature들은 뒤에서 해석을 확인해볼만하다. 

R2가 낮은걸 뽑아보면 luad - Neutrophil_signature(meth 0.104, RPPA 0.067), Angiogenesis(RPPA 0.165, 메틸화 0.130)이고 lusc - M1_signatures(meth 0.183, RPPA 0.080)였다. 

아무튼 그래서 결과가 시사하는바는 subtype별로 laud는 rppa, lusc는 methylation이 채택되는 경향이 있고 replicate수는 선택이 전혀 되지 않았다.

++)

R2는 0.35가 나오면 T cell activation 점수가 환자마다 다른 정도 중 35%를 methylation을 맞혔다는 것이고 나머지 65%는 methylation으로 알수없다는 의미이다.

보조 정답은 정답에 없는 새 정보를 담고 있는게 아니라 정답(T cell activation)를 methylation이 보는 방향으로 비춘 결과이다. 정답 중에서 methylation과 겹치는 부분만 남기고 나머지를 지운 결과이다. 이 보조 정답을 넣어주는것의 의미는, RNA가 T cell activation을 판단할때 RNA가 담고 있는 정보 중 보조 오믹스도 동의하는 부분은 더 강조되고, RNA만으로 알수없는 정보 중 보조 오믹스가 담고 있는 정보도 추가로 볼수있다. 

다만 보조 오믹스 채택 자체를 RNA 점수를 맞히게 채택했기 때문에 보조 오믹스가 본 T cell activation 중 RNA도 동의하는 부분만이 추가된다. 그래서 보조 오믹스 추가에 따라 성능이 크게 오르는것은 기대하기 어렵고 보조 오믹스 추가에 따라 모델의 attention 이 어떻게 달라지는지를 통해 잡음을 개선할수있다. 

그리고 tumor purity처럼 RNA와 methylation이 함께 동의하는 신호라면 즉 두 정답이 모두 틀리는 부분은 걸러지지 않고 오히려 강조돼버린다. 

그리고 또다른 성능이 오르기 어려운 이유는 평가를 여전히 RNA 측면에서 하기 때문이다. 모델이 T cell activation을 더 잘 학습하고 근본적으로 더 잘 맞히게 되어도, 성능은 RNA score와의 상관으로 재기 때문에 그 개선의 일부만 성능 수치로 드러나게 된다. 

### 1-7. attention head 그룹화

Histotme 연구에서는 attention head를 Bagaev 연구의 4 group으로 묶어서 성능이 약간 좋아짐을 확인했다. 

우리 연구에서는 Bagaev 4 group 외에도 data based group을 정의해서 전체 그룹화 및 개별 head 사용과 성능을 보기로 했다. 

grouping 방식은 g0(signature마다 head 할당), g2(Bagaev 연구의 4 group), gr(g2의 무작위 버전), g6(coexpression 된 signature 끼리 클러스터링, k=4), g9(변동이 큰 top 5000 genes로 pc 분석하여 pc1 2 3을 정하고 상관이 가장 큰 pc축에 배정하고 어느쪽도 아니면 other로 배정, k=4인 셈)으로 정했다.

추가로 g0으로 학습한 후 29개의 attention map을 만들고 같은 타일에 높은 attention을 줬으면 점수가 크도록 attention similarity matrix를 만들어서 그걸로 k=4 로 grouping했다. (g7)

그리고 attention head를 어떻게 묶을지를 안정하고 4개를 둔 다음에 자동으로 grouping 하게하는 방법도 있다. (g8) (M4: Multi-Proxy Multi-Gate Mixture of Experts Network for Multiple Instance Learning in Histopathology Image Analysis 논문에서 사용한 방법)

이렇게 grouping을 짰는데, 성능을 보기 전에 각 grouping 방식마다 그 grouping의 각 group의 구성이 어떻게 되고, rna 값이 group 내에서 얼마나 같고 group 간에는 얼마나 다른지 등을 확인했다. 그리고 grouping의 이득과 손실도 계산했다.

먼저 g6은 silluette score로 자르니까 k=2가 나왔는데 크기가 [28,1]로 Proliferation이 떨어졌다. 그리고 k=4로 돌리면 luad - 18/7/3/1, lusc - 18/3/7/1이 나왔다.

g9는 luad에서는 pc1/2/3/other이 각각 8/8/10/3이 나왔고 lusc에서는 25/0/4/0으로 pc1에 몰렸다. (이거 subtype 별로 돌린거 맞는지 확인 필요)

그리고 group 내 결속을 봤을때 무작위인 gr에서 이미 0.385/0.422(luad/lusc)로 좀 높게 나왔다. 각 group에 대해서 gr과의 결속력 차이를 봤을때 g6 k=4버전이 0.35/0.37로 제일 컸고 g2 0.21/0.22, g2 0.19/0.18, gr(-0.04~~0.04/-0.02~~-0.08)로 나왔다. 

위 수치는 group들의 평균을 내서 각 grouping의 결속력을 평균적으로 본것이고 grouping 방법 안에서 각 group 별로 보면 결속이 약한 group도 있었는데 g2의 n 크기 group은 0.20/-0.23이었고 g2의 n 크기 group은 0.44/0.52로 작게 나왔다. 

그리고 거의 모든 grouping에서 각 head에 할당된 보조 오믹스가 동일했다. luad는 모든 head의 보조 오믹스가 rppa였고 lusc는 g9 pc3 head 제외하고는 모든 head의 보조 오믹스가 methylation이었다.

여기서 알수있는것들은

1-무작위로 묶어도 (gr) group 내 결속력(상관)이 0.36~~0.48로 꽤 높다. 29 signature를 하나로 묶은 g1도 0.385/0.422이다. 그 이유는 29 signature가 모두 미세환경 시그니처여서 같은 흐름을 따르기 때문일수있다. 종양에 면역세포와 기질세포가 많은 환자는 t cell, b cell, macrophage, caf score가 다같이 높고, 종양세포가 많은 환자는 다 같이 낮다. 즉 signature마다 세부 내용이 달라도 미세환경 세포가 얼마나 많은가 라는 공통 축을 같이 따른다. 실제로 g6 에서 silluette score 기반으로 grouping 했을때 [28,1]이 뚜렷하게 나왔다.
여기서 알수있는건 signature들이 이미 큰 축을 공유하고 있기 때문에 어떻게 묶든 한 head 안의 signature들이 비슷한 학습신호(환자간차이신호)를 보려고 할것이다. (비슷한 부분을 보려고 하는건 아니겠지만). 그러므로 묶는 방식을 바꾸면 모델이 attention한 부분이 다르더라도 성능 차이는 크게 나오지 않을 것으로 예상할수있다. 

2-결속력이 약한 group은 attention을 공유하는것이 오히려 성능을 떨어뜨릴수있다. 한 group에 할당된, 즉 한 head를 할당받은 signature 끼리는 슬라이드에서 어느 타일을 attention할지 같이 정하는데 두 signature가 같은 방향으로 움직이면 같은 부분을 보면 되니까 공유가 쉽지만 반대 방향으로 움직이면 한 head가 서로 다른곳을 동시에 봐야해서 타협이 생겨서 둘다 성능 면에서 손해를 볼수있다. 이 부정적 전이가 나타날걸로 생각되는게 g2의 n=2 group인데, Proliferation_rate, EMT_signature이고 group 내 상관이 0에 가깝거나 음수여서 proliferation이 높을수록 emt가 낮은것이다. proliferation은 종양세포가 많을수록(purity가 높을수록) 높고 emt는 기질이 많을수록 높기 때문. 즉 두 signature의 단서는 도메인 지식만 보더라도 슬라이드에서 각각 종양실질/기질로 다른곳을 보는게 맞기때문에 한 head로 동시에 맞히기 어렵다. g2의 n=11 group은 group 내 상관이 0.44/0.52로 낮진 않지만 타 group 과의 상관도 0.42/0.52여서 group 내와 group 간이 비슷하다. 그래서 grouping 되었지만 뭉쳐있지 않다 즉 그 group의 기능 분류는 rna에서 하나의 덩어리로 보이지 않는다. 

+) grouping의 이득과 손실 계산: luad 의 G2 방식에서 첫번째 group에 속한 시그니처가 11개인데, 각 11개 시그니처마다 보조 오믹스가 채택되어있다. 이 11개는 head 하나를 사용하는데 연구 설계상 head당 보조 오믹스를 하나 사용하여야한다. 그래서 group 마다 보조 오믹스를 채택해줘야한다. group의 보조 오믹스를 고르는 방식은 현재 group 내 시그니처들의 R2 값을 평균해서 가장 높은 R2인 오믹스를 채택하는 방식으로 정했다. 

이렇게 group 단위로 보조 오믹스를 골랐을때 a 시그니처가 가장 최적인게 methylation이라도 RPPA의 보조 정답을 받는 식으로 학습되어버린다. 이 grouping 비용은 두가지로 측정 가능한데 첫번째는 각 시그니처마다 grouping 후의 R2 값을 계산해서 G0에서의 단독 R2값과의 차이로 측정할수있다. 두번째는 현재 group 안 시그니처 중 자신 단독으로 최적인 오믹스가 실제 지금 보조 오믹스로 선택된 비율을 계산하면 된다.    

이상적으로는 grouping을 했을때 각 group 안의 모든 시그니처가 하나의 오믹스를 최적으로 보면 두번째 값이 1.0이 된다. head에 보조 오믹스 하나만 붙이는것의 손해가 없다. 

반대로 무작위로 grouping을 했을때는 모든 시그니처가 반대 오믹스를 고르는것보다는 좋고 이상적인 grouping 보다는 손해가 된다. 이렇게 묶은 GR을 만들어서 grouping의 이득과 손실을 ~ 측면에서 계산했다. 

즉 특정 group의 fit mean R2와 layer agree ratio가 GR보다 높으면 그 group은 선호가 비슷한 시그니처끼리 묶인 상태니까 보조 오믹스가 head 단위로 선택된것에 따른 손해가 적은 것이다.
