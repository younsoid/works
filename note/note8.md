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

G6는 RNA 공발현으로, G7은 G0 attention의 형태 유사도로 시그니처를 묶었다. 같은 fold의 학습 환자(test 제외)에서 시그니처 쌍 406개마다 RNA 상관과 형태 유사도를 구해 5개 fold 평균을 냈다. 두 값의 Spearman 상관은 LUAD 0.58, LUSC 0.70으로, RNA로 같이 움직이는 쌍일수록 attention도 비슷한 경향이 있었다. 

이 경향에서 벗어난 쌍을 찾으려고, 406쌍에 형태 유사도 ~ RNA 상관 직선을 맞추고 잔차를 표준화한 z가 ±2를 넘는 쌍을 골랐다. LUAD에서 31쌍(경향보다 attention이 덜 비슷한 쪽 23, 더 비슷한 쪽 8), LUSC에서 26쌍(14, 12)이었다.

<img width="5373" height="2584" alt="fig10_rna_vs_attention_pairs_LUAD" src="https://github.com/user-attachments/assets/7f4afc99-bd0d-47ab-a58e-1f17d0b371fb" />
<img width="5373" height="2584" alt="fig10_rna_vs_attention_pairs_LUSC" src="https://github.com/user-attachments/assets/35523b75-ba17-460d-a84c-64b836f44ebd" />

첫째는 RNA로 같이 움직이는 정도에 비해 attention이 덜 비슷한 경우다. 같은 RNA 축에 있어도 모델이 그 시그니처를 맞히려고 보는 부위가 다른 경우다. LUAD에서는 호중구가 가장 뚜렷했다. 덜 비슷한 쪽 23쌍 중 18쌍이 Neutrophil_signature 짝이었고, 이 시그니처가 낀 쌍 28개의 평균 잔차는 −0.28로 가장 낮았다. Granulocyte_traffic과는 RNA 상관이 0.42인데 형태 유사도는 0.10(z −3.0), MDSC_traffic과는 RNA 0.48에 형태 0.17(z −2.7)이었다. 다음은 Endothelium(평균 잔차 −0.23)이다. Endothelium은 혈관신생, CAF, 세포외기질과 함께 5개 fold 모두 G6 기질 그룹에 들어갔다. Angiogenesis와는 RNA 0.74에 형태 0.34(z −2.2), CAF와는 RNA 0.60에 형태 0.29(z −2.1)였다. B 세포도 평균 잔차가 −0.13으로 낮은 쪽이었다. 다만 T 세포와의 개별 짝(RNA 0.70, 형태 0.49)은 기준에 들 만큼 벗어나지 않아, 특정 짝보다는 시그니처 전반에 걸친 약한 경향이었다. LUSC에서는 Endothelium(−0.079)과 함께 MHCII(−0.079), Treg(−0.067), B_cells(−0.066) 같은 림프구 계열이 낮은 쪽에 있었다. 주로 호중구 계열·혈관 시그니처와의 짝이 경향보다 덜 비슷했다(MHCII–Granulocyte_traffic z −2.7, MHCII–Neutrophil z −2.6). LUSC에서 같은 G6 그룹인데 attention이 가장 다른 짝은 MHCII–T_reg_traffic이었다(RNA 0.62, 형태 0.27, z −2.1).

둘째는 RNA로는 따로 움직이는데 attention은 경향보다 더 비슷한 경우다. 두 조직형 모두 Proliferation_rate가 대부분을 차지했다. LUAD에서는 더 비슷한 쪽 8쌍이 모두 Proliferation_rate 짝이었고, 평균 잔차는 +0.21이었다. T_reg_traffic과는 RNA −0.24인데 형태 0.72(z +3.2), Th2_signature와는 RNA −0.24에 형태 0.68(z +2.9), Checkpoint_inhibition과는 RNA 0.11에 형태 0.73(z +2.4)이었다. LUSC에서도 12쌍 중 10쌍이 Proliferation_rate 짝이었다(평균 잔차 +0.11). EMT_signature와는 RNA −0.23에 형태 0.50(z +3.0), MDSC_traffic과는 RNA −0.36에 형태 0.44(z +2.8)였다. 06-4에서 증식은 나머지 28개와 RNA로 함께 움직이지 않는 유일한 시그니처였지만, attention에서는 RNA가 시사하는 만큼 따로 보지 않았다. 생물학적으로는 독자적인 신호인데, 모델은 이를 맞힐 때 고유한 부위보다 다른 시그니처들이 함께 보는 영역을 보는 것으로 보인다. LUSC에서는 이 밖에 RNA로 아주 강하게 묶이는 T·NK 세포 짝도 경향선 위에 있었다. Effector_cells–NK_cells는 RNA 0.94에 형태 0.70(z +2.4), T_cell_traffic–T_cells는 0.86에 0.66(z +2.1)이었다. RNA로 강하게 묶이는 면역 세포 짝은 attention에서도 실제로 같은 곳을 본다는 대조 사례다.

정리하면, RNA 공발현으로 묶는 것과 attention 형태로 묶는 것은 대체로 같은 방향이지만 의미가 다르다. 그 차이는 특정 시그니처에 몰려 있다. RNA에 비해 attention이 따로 노는 쪽은 LUAD의 호중구·혈관 내피·B 세포, LUSC의 혈관 내피·림프구 계열이다. RNA에 비해 attention이 공통 영역을 보는 쪽은 두 조직형 모두의 증식이다. 앞의 시그니처들은 08-1에서 G7 큰 무리 밖으로 자주 떨어져 나온 시그니처와 겹친다. 다만 z ±2 기준은 결과를 보고 정한 값이고, 직선 하나로 경향을 잡은 것이라 강조 쌍의 경계는 기준에 따라 달라질 수 있다.
