### 7-3 기준선: 슬라이드 평균 특징 + 릿지

G0, 2, 6, 7마다 seed 0·1·2를 각각 기준선(lidge 회귀 결과)와 비교하고 평균 ± SD를 냈다. 괄호 안은 Δr = r(기준선) − r(설계)로, 음수이면 설계가, 양수이면 기준선이 나은 것이다.

설계 4개(G0·G2·G6·G7) 평균 r은 LUAD/LUSC에서 0.378/0.321로, 기준선 0.357/0.327과 비교해 LUAD에서는 0.020 높고 LUSC에서는 0.006 낮았다(Δr −0.020 ± 0.012 / +0.006 ± 0.004, 설계 간 SD).

G0 단일 모델은 seed 3개 평균 0.359/0.326으로 기준선과 비슷했다(Δr −0.002 ± 0.022 / +0.001 ± 0.014). seed에 따라 0.341~0.384/0.315~0.342로 흔들려, 기준선보다 낮기도 높기도 했다.

G2, G6, G7은 LUAD에서 0.381, 0.383, 0.387로 기준선보다 높았다(Δr −0.024 ± 0.008, −0.025 ± 0.002, −0.030 ± 0.006). LUSC에서는 0.316, 0.320, 0.323으로 기준선과 사실상 비슷했다(Δr +0.011 ± 0.011, +0.006 ± 0.003, +0.004 ± 0.015).

시그니처별로는 seed 3개 평균 ± SD가 0을 포함하지 않는 것만 셌다. 설계 4개 평균으로 보면 LUAD는 기준선 우위 5개, 설계 우위 16개였고, LUSC는 각각 11개, 10개였다.

LUAD에서는 CAF, EMT, Matrix에서 기준선이 모든 설계보다 나았다(설계 4개 평균 +0.077, +0.054, +0.034).
- G0: CAF +0.114, EMT +0.048, Matrix +0.041, Th2 +0.021
- G2: CAF +0.061, B_cells +0.057, EMT +0.048, Granulocyte_traffic +0.035, Matrix +0.035
- G6: B_cells +0.079, CAF +0.059, EMT +0.055, Granulocyte_traffic +0.032, Matrix +0.032
- G7: CAF +0.075, EMT +0.064, B_cells +0.051, Granulocyte_traffic +0.035, Matrix +0.030

묶은 설계에서는 B_cells와 Granulocyte_traffic도 기준선이 나았다(설계 4개 평균 +0.053, +0.033).

LUSC에서는 Treg, T_reg_traffic, Coactivation_molecules, Checkpoint_inhibition에서 기준선이 모든 설계보다 나았다(설계 4개 평균 +0.087, +0.083, +0.074, +0.056).
- G0: Coactivation_molecules +0.075, Treg +0.074, T_reg_traffic +0.071, Checkpoint_inhibition +0.060, Macrophages +0.040, Th1 +0.028
- G2: Treg +0.104, T_reg_traffic +0.088, Macrophage_DC_traffic +0.084, Coactivation_molecules +0.077, Checkpoint_inhibition +0.076 외 4개
- G6: Macrophage_DC_traffic +0.083, Treg +0.081, T_reg_traffic +0.080, Coactivation_molecules +0.078, Antitumor_cytokines +0.051 외 3개
- G7: T_reg_traffic +0.093, Treg +0.089, Macrophage_DC_traffic +0.076, Coactivation_molecules +0.067, Checkpoint_inhibition +0.044 외 2개

반대로 기준선이 뒤진 것은 LUAD에서 T_cell_traffic, Neutrophil, Macrophage_DC_traffic, Endothelium이었다(설계 4개 평균 −0.103, −0.088, −0.073, −0.072).
- G0는 7개: Neutrophil −0.108, Macrophage_DC_traffic −0.067, T_cell_traffic −0.066, Endothelium −0.066, Antitumor_cytokines −0.050 등
- G2·G6·G7은 각 17개로, 림프구·대식세포 계열이 많았다. T_cell_traffic −0.096~−0.124, Neutrophil −0.070~−0.091, Endothelium −0.065~−0.079, Treg −0.050~−0.074

LUSC에서는 Protumor_cytokines, Endothelium, CAF, Angiogenesis에서 기준선이 뒤졌다(설계 4개 평균 −0.060, −0.058, −0.050, −0.036). 설계마다 5~7개였다.
- G0: Endothelium −0.081, MDSC_traffic −0.072, Protumor_cytokines −0.062 등
- G2: CAF −0.065, Protumor_cytokines −0.064, Endothelium −0.063 등
- G6: Protumor_cytokines −0.056, Neutrophil −0.054, Angiogenesis −0.049 등
- G7: Protumor_cytokines −0.059, CAF −0.056, Angiogenesis −0.050 등

시사점은 세 가지다.
- 단일 모델 기준으로 G0는 평균 특징 + 릿지보다 낫지 않았고, 묶은 설계도 LUAD에서만 조금 나았다. attention 모델의 가치는 전체 성능이 아니라 "어디를 보는가"를 알려 준다는 데 있다.
- 혈관 내피는 두 조직형 모두에서, 호중구는 LUAD에서 attention 모델이 일관되게 나았다. 슬라이드 안에 국소적으로 드물게 있는 세포는 특정 타일을 골라 보는 방식이 필요하다는 해석과 맞는다.
- 기준선이 나은 시그니처는 조직형마다 달랐다(LUAD 기질 계열, LUSC 조절 T 세포·면역 관문 계열). CAF는 LUAD에서는 기준선이, LUSC에서는 attention 모델이 나았다. 따라서 "조성 시그니처는 평균으로 충분하다"는 조직형을 넘어 일반화하지 않는다.
