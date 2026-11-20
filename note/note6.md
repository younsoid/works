# 9. 평가

### 9-1. 기본 성능

grouping은 g0, g2, g6, g7, g9로 진행했다. 성능 확인결과 성능 간 차이는 적고(0.40~~0.42), baseline(0.357)보다는 모두 성능이 높다. (평균 R 기준)

그리고 seed를 3번 했는데 seed간 차이가 많으면 모델 강건성이 낮아서 잘 흔들린다는 것이고 단일 seed와 3-seed 를 공평하게 비교하면 안된다는 것이다. 

확인 결과 3-seed를 하면 R값이 커지는것을 확인해서 3-seed까지 다 돌리고 확인해야 공정하다. 

### 9-2 grouping 이득 확인 (luad)

첫번째로 확인해야할것은 grouping했을때 나아졌는지이다. grouping 안한 셈인 g0과 비교해서 g2, g6, g7, g9의 성능R의 차이를 본다.

g0과의 차이 Δr을 luad에서 확인한결과 fdr 보정 후 G9 +0.023, G7 +0.023, G6 +0.016, G2 +0.016으로 성능이 조금 개선되긴했으나 증가 폭은 크지 않다.

g0과의 차이 Δr은 모든 signature에 대해 평균값이다. 어떤 signature는 이득을 봤음에도 다른 signature의 손해로 효과가 드러나지 않았을수 있으므로 signature별로 g0과의 차이 Δr를 확인했을때 유의하게 오른것은 림프구 계열이었다:Th1, MHCI, T_cell_traffic(G6·G7·G9), Treg(G6·G7), Effector_cells(G7). 그 밖에 MDSC·MDSC_traffic(G9), CAF(G6).유의하게 내린 것은 G6의 B_cells(−0.077) 하나다.

알수있는점은 먼저 grouping 방법에 따라서 성능이 크게 차이나지는 않다. g7, g9가 성능개선이 조금 높은편이긴하다.   

그리고 특정 signature에서 성능이 오른게 아니라 17 signature가 4 grouping 모두에서 성능이 올랐다. 

유의하게 오른 signature들은 Th1, mhc1, T cell immigration, Treg, T effector cell 등 림프구 계열에 몰려있다. 이 signature들은 g6에서 면역 group에 속한 signature 들로,  co expression 되면서 attention을 공유했을때 서로 긍정적 전이가 있는 경우라고 볼수있다.  (co expression이라고 해서 같은 부분을 봤을때 이득인것은 아니니까)

(그럼 연구의 목적은 co expression과 attention이 같은 것 이렇게 2 축으로 signature들을 나누는 것. 그리고 후속 분석.)

모든 grouping에서 부정적 전이가 있었던 signature 5개 는 Neutrophil·Granulocyte_traffic, EMT, B cells였다. RNA 맞히는것이 목적인 만큼 RNA 가 co expression 되지 않은, 그래서 면역/기질이라는 큰 group에 속하지 않은 signature들이 역시나 다른 signature와 grouping 했을때 손해를 본다. (그러면 G6에서는 성능이 올랐어야 하는거 아닌가? 근데 성능 자체는 크게 변화 없을것으로 예상하긴 했음. 낮아졌다기보단 변화없다고 보는게 맞을것같다. )

부정적전이가 g2에 있지 않을까 생각했는데 g6의 b cells 제외하고는 부정적 전이는 없었다. 

### 9-3. grouping 이득 확인 (lusc)

g0과의 차이 Δr을 luad에서 확인한결과G6 +0.003, G7 −0.006, G2 −0.013, G9 −0.022으로 성능이 거의 변화가 없고 오히려 약간 낮아졌다. grouping 했을때 성능이 낮아졌다는것은 앞선 분석에서 lusc에서는 큰 미세환경축 하나를 signature들이 따른다는 것을 보여준다. 

내린 signature가 grouping마다 17–24개다. 모든 grouping에서 내린 것이 11개(Treg, MHC 계열, 기질 등), 모두에서 오른 것은 3개(Angiogenesis, CAF, Macrophages)다. 유의하게 내린 것이 MDSC_traffic(G2, G9), T_cells·B_cells(G9)이다. 

