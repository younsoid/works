### 6-3. k=2 g6는 사실상 g1이다. 

k=2 g6의 group 내 ρ는 luad 0.42, lusc 0.47로, g1의 group 내 ρ(0.39, 0.42)와 거의 동일하다. 

이 grouping으로 알수있는것은 rna coexpression이 큰 면역축 하나가 지배한다는것밖에 없다. data based grouping으로 새로운 축을 확인하려면 28 signature 안에서 유의미한 cut이 필요하다. 따라서 silluette score가 두번째로 높은 k=4 버전의 g6을 확인해본다. 

### 6-4. k=4 g6 grouping 개요

k=4로 grouping 한 결과 luad, lusc가 유사하게 나왔다. [18,9,1,1] [17,9,2,1]

구성도 유사하게 나왔는데 각각 면역/기질/neutrophil/proliferation 으로 구성돼있었다. 

즉 29 signature의 큰 축은 미세환경 세포수/proliferation 이었고 28 signature의 큰 축은 면역/기질인것으로 나왔다. 

면역 group은 보통 림프구, 대식세포 관련 signature, 기질 group은 섬유모세포, 콜라겐, 혈관 관련 signature이 들어있다.

해당 grouping과 g2의 Bagaev grouping 사이 연관성도 확인해봤다. (IE, IE/F, F, D)

면역 group은 IE와 IE/F가 0.8–0.95, D가 −0.8, F가 0정도 나왔다. 기질 group은 F와 IE/F가 0.6–0.9로 높게 나왔다. 

neutrophil group은 면역세포여서 IE에서 높아야할것같지만 매우 낮았다. 대신 F에서 가장 높았다. 즉 기질 group과 같이 움직이는 경향이었다. neutrophil은 섬유화가 있는 조직에 나타나는것으로 데이터상에는 나온것이다. 

proliferation group은 기질 group과 정반대로 나왔는데, IE/F와 F에서 매우 낮았고 IE와 D에서 가장 진했다. 면역 group과는 상관이 없었다. 그래서 proliferation group은 섬유화가 없는 조직의 특징으로 볼수있다. 

lusc, luad 각각 면역 group과 기질 group의 group 내 ρ, group 간 ρ를 확인해보면 group 내 ρ가 둘다 확실히 group 간 ρ보다 컸다. 즉 면역 group과 기질 group은 서로 상관이 0은 아니지만 group 내 경향성이 강하다. 그리고 laud 는 group 간 ρ가 0.26으로 lusc(0.36)보다 낮아서 좀더 group의 경향성이 강하다. 그래서 g6 k=4 로 attention을 묶는것이 laud에서 학습 이득이 더 클것으로 보인다. ρ가 작아서 두 group이 많이 따로 움직일수록 head 하나가 얻는 학습 이득, 즉 원래는 2개의 다른 부위를 보면서 타협(ex. 림프구 밀집 부위와 섬유화 부위 등)해야 할것을 따로 보게 됨으로써 head가 자기 근거만 보게 되므로 학습 이득이 커질것이기 때문이다. 한마디로 면역과 기질을 다른 head에 두는 설계의 이득이 laud에서 조금더 클것으로 생각된다. (많이는 아님. 0.36과 0.26의 차이여서,)

그리고 Bagaev의 결과와도 어느정도 맞아떨어지는것을 볼수있다. Bagaev는 29 signature를 면역침윤/섬유화/D로 설명했는데 우리가 signature correlation으로 구한 그룹도 면역/기질(호중구)/반기질(proliferation) 패턴으로 보면 비슷한 패턴을 띤다.

여기에 더해서, proliferation을 D의 특징으로 해석하기 쉽다. 보통 proliferation을 "종양세포가 많고 미세환경이 빈약한 D의 특성"이라고 본다. 즉 proliferation을 D 하나를 찾는 특성으로 이해하기 쉽다. 근데 현재 signature correlation 결과 측면에서 보면 IE에서도 proliferation이 높다. luad는 D보다도 IE에서 높다. 반대로 섬유화가 있는 group에서는 낮다(IE/F와 F) 즉 proliferation이 높으면 D 특성이라고 보는것보다 proliferation이 높으면 "섬유화없음" 특성에 더 가깝다는것을 볼수있다. 즉 D group을 담당하는 특성이 아니라 각 group을 섬유화있음과 없음으로 나누는 특성임을 확인할수있었다. 

이 경계는 group 경계중 가장 모호한 경계였다. 원 논문에서는 아직 확인 안해봤지만 Bagaev 재현때 confusion matrix에서 IE/F와 F, D 사이에서 가장 유출이 심했다. 이 경계를 더 명확히 하는 새로운 특성 proliferation을 사용하면 분류가 더 명확할수있다. 

이게 유의미한 이유는 g2의 grouping에서 찾을수있다. g2의 grouping은 proliferation과 EMT를 하나의 head로 보는데, 두 신호는 우리 분석에 따르면 같이 움직이지 않는다. rna가 같이 움직이지 않는다는것이 두 group의 근거가 다른 곳이라고까지는 해석할수없다. 하지만 우리의 새로운 분석에서 EMT는 섬유화 group, proliferation은 비섬유화 group으로 움직인다는것을 확인했다. 즉 EMT의 근거는 섬유화 부분에 있을것이고 LUAD proliferation의 근거는 비섬유화 부분 즉 종양세포 자체에 있을 가능성을 확인했다. 만약 실제로 근거가 다른곳에 있다면 한 head가 두 signature를 맞히게 한 g2 접근에서는 부정적 전이가 왔을 것이다. 그리고 g6(k=4)에서는 proliferation 을 담당한 head가 다른 signature와 타협 없이 proliferation에 맞는 근거만 볼수있게 된다. 이렇게 proliferation을 따로 둔 설계를 통해 proliferation에 대한 모델의 attention이 좋아지면 subtype 재현도 더 좋아질수 있고 그 차이가 IE vs IE/F 재현 성능에서 확인할수있을것이다. 

