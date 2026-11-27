### 6-2. G2

우선 29 signature 자체의 특성은 Bagaev 연구에서 언급하길 아래와 같다.
- 항원 제시 - MHCI, MHCII
- T cell activation - Coactivation_molecules, Effector_cells, T_cells, T_cell_traffic, Th1_signature, NK_cells, Antitumor_cytokines
- B cell - B_cells
- 대식세포(항종양) - M1_signatures
- 면역 억제 - Checkpoint_inhibition, Treg, T_reg_traffic, Th2_signature, Protumor_cytokines
- 골수계(억제) - Macrophages, Macrophage_DC_traffic, MDSC, MDSC_traffic
- 과립구 - Neutrophil_signature, Granulocyte_traffic
- 기질 - CAF, Matrix, Matrix_remodeling
- 혈관 - Endothelium, Angiogenesis
- 종양세포 - Proliferation_rate, EMT_signature

먼저 G2은 Bagaev 연구의 생물학적 지식 기반 묶음이다. 항종양, 친종양, 혈관신생/섬유화, 악성세포 특성 으로 나누었다. 
- 항종양 - MHCI, MHCII, Coactivation, Effector, T_cells, T_cell_traffic, NK, B_cells, M1, Th1, Antitumor_cytokines (항원 제시 -> T, NK activation -> 세포독성 T cell과 보조하는 B cell, M1 cell. 즉 면역이 종양을 공격하는 상태)
- 친종양 - Checkpoint, Treg, T_reg_traffic, Th2, Macrophages, Macrophage_DC_traffic, MDSC, MDSC_traffic, Neutrophil, Granulocyte_traffic, Protumor_cytokines (면역 억제하는 관문, Treg, Th2 cell과 억제성 골수계인 M2, MDSC와 과립구로 구성. 면역을 누르는 상태)
- 혈관신생/섬유화 - CAF, Matrix, Matrix_remodeling, Endothelium, Angiogenesis (섬유아세포, 기질, 혈관으로 구성. 비면역 기질 관련)
- 악성세포 특성 - Proliferation_rate, EMT_signature (종양세포 자체의 성질)

### 6-3. G6

G6는 29 signature의 rna coexpression 기반 묶음이다. luad [18,9,1,1] lusc [17,9,2,1]로 1개 제외 동일하게 나왔다. (fold 0 기준)
- [18,17] - MHCI, MHCII, Coactivation_molecules, Effector_cells, T_cells, T_cell_traffic, Th1_signature, NK_cells, Antitumor_cytokines, B_cells, M1_signatures, Checkpoint_inhibition, Treg, T_reg_traffic, Th2_signature, Macrophages, Macrophage_DC_traffic (LUAD - MDSC)
- [9,9] - CAF, Matrix, Matrix_remodeling, Endothelium, Angiogenesis, EMT_signature, Protumor_cytokines, MDSC_traffic (LUAD는 Granulocyte_traffic (LUSC - MDSC)
- [1,2] - Neutrophil_signature (LUSC- Granulocyte_traffic)
- [1,1] - Proliferation_rate

크기순 첫번째 그룹은 항원 제시, T cell activation 전부, B cell, 대식세포(항종양)에 면역 억제 중 Checkpoint_inhibition, Treg, T_reg_traffic, Th2_signature와 골수계(억제) 중 Macrophages, Macrophage_DC_traffic가 포함되어서 면역 관련 signature이다. Hbag의 항종양 11개 전부에 친종양 중 림프구·대식세포 계열이 더해졌다. 환자 간에는 면역을 공격하는 신호와 누르는 신호가 함께 오르내린다. T cell이 많은 종양에서는 면역관문, Treg, Th2도 함께 많기 때문이다. 즉 이 그룹은 면역의 방향(공격·억제)보다 면역세포 침윤의 양을 나타낸다.

크기순 두번째 그룹은 기질과 혈관에 EMT_signature, Protumor_cytokines, 골수계 유인 신호(MDSC_traffic 등)가 더해진 그룹니다. Hbag의 혈관신생/섬유화 5개에 EMT와 TGF-β·IL6 계열 사이토카인, 골수계 유인 신호가 더해졌다. EMT는 종양세포의 성질로 분류되지만 RNA로는 섬유아세포와 함께 움직이고, 섬유아세포가 분비하는 사이토카인과 케모카인도 있다. 즉 섬유아세포가 많은 기질 환경을 나타낸다.

호중구 계열은 과립구가 면역 무리와 함께 움직이지 않고 따로 떨어진 것이다.

증식은 나머지 28개와 무관하거나 반대로 움직인다(평균 ρ LUAD −0.02, LUSC −0.26). 미세환경이 많은 종양일수록 종양세포 비율이 낮아 증식이 낮게 나오기 때문이다.

참고로 나머지 fold에서는 이렇게 나왔다. fold가 바뀌어도 바뀌는 시그니처는 LUAD 2개, LUSC 3개뿐이고, 모두 기질과 과립구 경계에 있는 골수계 유인 신호다.

LUAD: fold 0–3은 [18, 9, 1, 1]로 같고, fold 4만 [18, 7, 3, 1]이다. fold 4에서 Granulocyte_traffic과 MDSC_traffic이 기질에서 호중구 계열로 옮겨 간다.
LUSC: 두 가지 형태가 번갈아 나온다.
fold 1·3은 [18, 7, 3, 1]이다. MDSC가 면역으로, MDSC_traffic이 호중구 계열로 옮겨 간다.
fold 2·4는 [17, 10, 1, 1]이다. Granulocyte_traffic이 기질로 옮겨 가 호중구 계열에는 Neutrophil_signature만 남는다.

호중구 계열과 골수계 유인 신호가 fold마다 옮겨 다니는 이유는 이 시그니처들이 CXCL8·CXCL5·IL6 같은 유전자를 기질 쪽 시그니처(Angiogenesis, Protumor_cytokines)와 공유해서 공발현만으로는 경계가 애매해서였을수 있다.

6-4. G7

G7은 fold에 따라 크기는 비슷하지만 큰 그룹 밖 signature는 매번 달랐다.

luad는 [25, 2, 1, 1] 1번 [26, 1, 1, 1] 4번 나왔는데 Neutrophil, Endothelium이 5번, B_cells가 3번, Granulocyte_traffic (2), CAF (1) 번 attention 경향이 달랐다. 
- Neutrophil (5/5): 호중구는 괴사 부위, 폐포 안 염증 삼출물처럼 특정 장소에 몰려 있다. Granulocyte_traffic도 두 번 함께 떨어져 나왔다.
- Endothelium (5/5): 혈관 벽이라는 뚜렷한 구조. 같은 혈관 영역인 Angiogenesis는 매번 큰 무리에 남았다. 내피세포 자체는 형태로 보이지만, 신생혈관 "신호"(VEGF 등)는 여러 세포가 내기 때문에 특정 장소가 없다고 해석할 수 있다.
B_cells (3/5): 3차 림프 구조(TLS)처럼 B세포가 뭉친 여포 구조일 가능성이 있다. T세포는 종양 안팎에 넓게 퍼지는 반면 B세포는 덩어리로 모이기 때문이다.

lusc는 fold 1과 2에서는 기질 시그니처 6개(CAF, Matrix, Matrix_remodeling, Angiogenesis, EMT, Protumor_cytokines)가, fold 0, 4에서는 Treg, Macrophages, T_reg_traffic, Proliferation, MHCII 가 attention 경향이 달랐다. LUAD처럼 매번 떨어지는 구획은 없고, 비교적 자주 떨어지는 것은 기질 덩어리와 Endothelium·Angiogenesis 같은 혈관 계열이었다,

fold 간 안정성(ARI)은 LUAD 0.70, LUSC 0.13이다.

즉 head를 분리해 줘서 의미가 있는 시그니처는 공간적으로 국소적인 구획이다. 호중구, 혈관 내피, B세포 응집처럼 특정 장소에 모여 있는 세포가 독자적인 attention을 만든다. 반대로 T세포, 대식세포, 사이토카인처럼 넓게 퍼져 있거나 분비 신호인 시그니처는 공통 영역을 본다. 그래서 G7은 시그니처의 기능보다 그 세포가 조직에서 어떻게 분포하는가를 반영한 묶음이다.

추가로, LUSC에서는 attention 구조 자체가 재현되지 않았다. ARI 0.13은 학습 환자가 조금만 바뀌어도 G0 attention의 유사 구조가 달라진다는 뜻이다. LUSC G0 head들의 attention이 덜 안정적이거나, LUSC 조직에서 구획 간 형태 차이가 덜 뚜렷할 가능성이 있다. G7 그림 오른쪽에 넣은 G0 seed ρ 띠를 보면, 떨어져 나온 시그니처가 seed 재현성이 낮은 쪽이다 즉 떨어진 시그니처의 seed ρ가 낮으므로 그 분리는 잡음일 가능성이 높다. 반면 G6는 떨어져 나온 시그니처의 seed 재현성이 높다. G6의 분리는 잡음일 가능성이 낮다. 

