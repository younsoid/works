### 8-4. Attention similarity와 RNA coexpression

Attention similarity와 RNA coexpression이 같이 가는지 따로가는지를 grouping 기반으로 확인했는데, 구체적으로 경향성을 수치화하기 위해서 g6과 g7의 corrlation matrix내 수치 406개 (29x28/2)를 사용해서 Attention similarity와 RNA coexpression 사이 Spearman ρ을 fold별로 측정했다. 그리고 5 fold 평균값 406개를 사용해서 406개 중 표준화한 잔차 z가 ±2를 넘는 쌍을 경향에서 벗어난 쌍으로 식별하고 시각화했다. 

<img width="5373" height="2585" alt="fig10b_attention_pairs_LUAD" src="https://github.com/user-attachments/assets/8b4966fb-e215-450e-b35c-1f8820b4ca40" />
<img width="5373" height="2585" alt="fig10b_attention_pairs_LUSC" src="https://github.com/user-attachments/assets/614250bc-18c5-4ad6-828a-fad764eb5956" />

확인 결과 attention과 rna가 같이 움직이는 경향이 있다. (0.40-0.71) 

rna에 비해 attention이 grouping 되지 않은 경우는 laud에서 23 pair, lusc에서 14 pair 였다. 

luad 23 pair 중 18 pair가 Neutrophil이었다. 즉 Neutrophil은 RNA로 함께 움직이는 signature라 해도 모델의 attention 부위는 달랐다.

나머지 5 pair는 Endothelium - 기질 signature였다. 즉 Endothelium은 같은 기질 signature라 해도 Angiogenesis, CAF, EMT, Matrix, Protumor_cytokines 5개 signature와는 다른 부분을 본다.

lusc는 {면역 signature MHC2, B cells, Treg} - {Neutrophil·Granulocyte_traffic·Angiogenesis·Endothelium} 가 대부분이었다.

다음으로 rna에선 grouping 되지 않았지만 attention이 유사했던 경우는 laud에서 8 pair, lusc에서 12 pair 였다. 

luad 8 pair는 모두 Proliferation이었고 lusc 12 pair중 10 pair가  Proliferation이었다. 이는 Proliferation이 RNA에서는 반대로 움직였으나 Attention은 같은 곳으로 볼수도 있지만, Proliferation head가 grouping을 통해 자기 고유의 부위를 갖지 못하고 공통 부위에 휩쓸렸을 가능성도 있다. 


