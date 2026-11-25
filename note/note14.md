### 1-5. 보조 오믹스

1-5-1. 보조오믹스 선택

오믹스 데이터로는 변이(MAF), miRNA, methylation, rppa가 있었는데 이중 miRNA와 변이를 제외하고 methylation, rppa를 보조 오믹스 후보로 선택하였다. miRNA와 변이의 제외 이유는 RNA와 직접적인 연관성이 있다고 판단되었기 때문이다. 

1-5-2. 보조오믹스 전처리

rppa에 대해서 preprocessing을 수행했는데 laud lusc 모두에서 결측이 10% 이하인 antibody만 사용하였고 filtering 결과 468 -> 455개 antibody를 사용하였다. (제외한 13 antibody: AXL, BRCA2, CASPASE9, CTLA4, E2F1, EZH2, GATA6, NAPSINA, NRF2, P63, PDCD1, RET_pY905, TTF1)

여기서 NAPSINA와 TTF1, P63는 각각 laud, lusc 조직형 ptn이고 CTLA4, PDCD1(PD-1)는 immune checkpoint 관련 ptn, NRF2는 lusc 고빈도 변이 NFE2L2 변이와 직접 연결되는 ptn인데 제외되었다. (확인 필요)
