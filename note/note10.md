### 8-4. Attention similarity와 RNA coexpression

Attention similarity와 RNA coexpression이 같이 가는지 따로가는지를 grouping 기반으로 확인했는데, 구체적으로 경향성을 수치화하기 위해서 g6과 g7의 corrlation matrix내 수치 406개 (29x28/2)를 사용해서 Attention similarity와 RNA coexpression 사이 Spearman ρ을 fold별로 측정했다. 그리고 5 fold 평균값 406개를 사용해서 406개 중 표준화한 잔차 z가 ±2를 넘는 쌍을 경향에서 벗어난 쌍으로 식별하고 시각화했다. 

<img width="5373" height="2585" alt="fig10b_attention_pairs_LUAD" src="https://github.com/user-attachments/assets/8b4966fb-e215-450e-b35c-1f8820b4ca40" />
<img width="5373" height="2585" alt="fig10b_attention_pairs_LUSC" src="https://github.com/user-attachments/assets/614250bc-18c5-4ad6-828a-fad764eb5956" />

확인 결과 attention과 rna가 같이 움직이는 경향이 있다. (0.40-0.71) 

rna에 비해 attention이 grouping 되지 않은 경우는 laud에서 호중구, 혈관내피, B cell 이고 lusc는 혈관내피, 림프구 계열이었다. 즉 rna 발현량이 유사했지만 모델이 시그니처를 맞히려고 보는 부위가 달랐다. 

rna에 비해 attention이 grouping된 경우는 laud lusc 모두 증식이었다. 모델이 증식을 맞힐때 다른 signature와 같은 영역을 본다. 

