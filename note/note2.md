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
