# A Pathway-Structured Privileged Distillation Framework for Deployable Computational Pathology

<img width="759" height="188" alt="image" src="https://github.com/user-attachments/assets/352f9df1-e424-44c7-8de6-023fcebb9f49" />

wsi는 암 진단시에 항상 생성되는 데이터. 암세포의 생김새, 암조직의 구조, 면역 세포와 섬유 조직이 주변에 어떤 조성인지를 볼수있다.

wsi만 봐서는 유전자 발현량은 알수없다. 유전자 발현 점수로는 재발위험정보 등을 알수있다. wsi로는 알기 어렵다.

wsi-RNA 데이터가 모두 있으면 재발위험 등을 예측할수있겠지만 보통 wsi만 있다. wsi로 RNA를 예측하고 그 RNA로 재발위험등을 예측할수있을까? 아니면 wsi-RNA로 재발위험등을 예측해서 wsi의 어디를 어떻게 보면 되는지를 배워서 wsi만으로 재발위험등을 예측할수있을까?

wsi만으로 재발위험등을 예측할수 있으려면 일단 기본전제는 알고싶은 정보가 wsi에서 '숨겨져있지만 발굴은 된다'가 되어야 한다. 알고싶은 정보가 wsi를 어떻게 비틀어봐도 알아낼수없는 정보인거라면 의미없는 작업인것이다. 이를 부분관측성이라고 한다. 즉 wsi 사진으로는 알고싶은 정보의 일부만 보인다. 

wsi에서 '숨겨져있지만 발굴은 된다'는 것의 의미는 무엇인가? wsi에서 특정 유전자 발현량이 관찰되는게 아니지만 그 유전자 발현량이 남긴 흔적을 형태에서 읽는것이다. 

비유하면 흡연자인지 아닌지를 혈액속 니코틴을 측정한게 아니라 피부, 치아, 주름같은 흔적을 보고 판별하는것이다.

이런 흔적이 잘 남는 유전자 발현량은 맞힐수있지만 흔적이 남지 않는 경우는 wsi로 맞힐 방법이 없다. 세포 모양이나 배치를 바꾸는 분자 변화는 사진에 흔적을 남기지만, 모양을 바꾸지 않는 변화는 사진에 아무것도 남기지 않는다.

이경우에 모양을 바꾸지 않는 변화까지 맞히게 하면 불가능을 모델에게 강요하는것이고 모델은 답을 내기 위해서 생물학적으로 무의미한 패턴으로 답을 낼것이다. 그러므로 모양을 바꾸는 변화만 모델에 넣고 학습해야 그 모양을 바꾸는 변화라도 잘 맞힐수있고 모델의 해석가능성을 가지고 생물학적으로 유의미한 발견을 할수있다. 

우선 wsi+rna 로 학습시키고 wsi만으로 예측을 내는 모델의 기법으로는 3가지가 있다.

첫번째는 direct feature matching인데 input인 wsi 벡터와 gene expression 벡터의 차이를 줄이는 방식이다. 보통 벡터 사이 mse(l2 distance)나 cosine distance를 손실로 사용한다. wsi 벡터는 데이터마다 gene expression 벡터의 모든 차원을 그대로 재현하도록 학습된다.

두번째는 similarity preserving distillation인데 gene expression 벡터 자체를 맞히는게 아니라 샘플 사이의 관계를 맞힌다. gene expression 벡터로 샘플간의 유사도 행렬을 구하고, 똑같이 wsi 벡터로 유사도 행렬을 구해서 그 행렬의 차이를 재현하는것이다. 

세번째는 subspace alignment인데 두 modality의 feature를 공통 잠재 부분공간으로 사영하고 그 공간 안에서 두 표현이 겹치도록 정렬하는 것이다. 이 논문에서는 gene expression 정보를 여러 부분공간으로 나누고 wsi 정보가 각 부분공간을 따라가게 하는 subspace knowledge distillation을 사용했다. 이 계열로는 CCA 같은 상관 기반 정렬이나 contrastive alignment등이 있다.

세번째 방식은 좋지만 RNA의 정보 전체를 wsi 표현으로 재현할수있다는 가정이다. 그러나 rna 정보 중에는 형태와 상관없는 성분도 분명 존재한다. 손실 함수가 이런 부분까지 맞히게 학습하면 그 성분을 맞히지 못하거나 우연히 상관되 엉뚱한 이미지 특징(기관 차이)같은 shortcut으로 맞히게 된다. (기존 방법들이 molecular representations를 fully transferable to histology로 암묵적으로 다룬다)

본 논문의 mope는 벡터를 직접 같게 만드는 대신 공유 기억 기저를 사용하는 패턴만 비슷하게 만드는 memory usage distillation을 사용한다. 즉 세번째 방식을 사용한다. 그러면서 gene expression 전체를 증류해서 high dimensional target을 맞히게 하는게 아니라, 더 알맞은 unit of supervision을 사용하는데 이를 MSigDB Hallmark gene set을 사용한다. pathway gene set은 개별 gene 들을 작고 해석 가능하면서, 증식이나 stromal remodelling같이 형태와 상관있는 target으로 만든다. 즉 pathway를 unit of supervision으로 사용해서 예측 대상으로 pathway를 설정하면 rna expression을 예측할때 생물학적으로 의미있으면서 형태와 상관있는 형태로 조직하면서 학습할수있다. 

즉 차원 축소(20000 -> 50)가 이뤄지고 형태와 연결성이 높아지고 해석 가능성도 높아진다. 

<img width="768" height="301" alt="image" src="https://github.com/user-attachments/assets/9ffcde3a-4178-4553-b9f5-85d0be4b4bc0" />

mope 모델은 2개 branch로 구성되는데 wsi branch, rna branch가 있다. rna branch는 학습할때만 존재한다.

rna branch에서는 샘플의 hallmark pathway 50개를 input으로 받고, 각 pathway k에 속한 gene expression을 합친 벡터를 선형 투영, layer norm 해서 pathway token 벡터를 만든다. 즉 샘플당 pathway token 50개가 생긴다.

wsi branch는 각 slide의 patch feature를 뽑고, 50개의 pathway indexed experts를 둔다. 각 expert 안에 morphology slot이 L개 있고 slot은 patch feature와 내적, softmax를 거쳐서 patch들의 attention 분포를 만들어, 각 패치를 weighted 평균한다. L개 두는 이유는 한 pathway가 여러 형태로 나타날수있기 때문이다. 예를들어 "염증반응" pathway면 림프구가 뭉친곳, 대식세포가 흩어진곳처럼 서로 다른 모양의 patch가 모두 근거가 될수있다. L개 slot의 결과를 합치면 경로 k에 대한 최종 expert token이 나온다. (즉 경로마다 attention head를 여러개 둔 셈이다)

<img width="382" height="229" alt="image" src="https://github.com/user-attachments/assets/d6e08b78-ff76-4f04-893c-c95349d00061" />

그러면 이제 pathway 마다 pathway token과 wsi expert token 이 있는데 어떻게 학습할것인가? 직접 배우게 하려면 cosine distance를 줄이거나 mse를 줄이면 된다. 이 논문은 rna token에 담긴 정보를 wsi에서 모두 확인할수없을수 있기 때문에 이를 반영해서 memory usage distillation 을 사용했다. rna와 wsi가 모두 쓰는 공용 벡터 집합인 memory bank C를 하나 두고, 안에는 M개의 잠재 기저 벡터가 있다. 이를 wsi feature을 k means로 clustering 한 centroid로 초기화한후 rna token을 M개 기저벡터 각각과 내적, softmax한다. 그러면 rna token이 어떤 기저벡터와 얼마나 닮았는지를 나타내는 확률분포가 나온다. 똑같이 wsi token으로도 확률분포를 만든다. 그리고 두 분포 사이 KL 발산을 줄여서 증류 손실을 낮춘다. 이렇게 하면 두 token이 같아지도록 학습하는게 아니라 같은 기저를 비슷한 비율로 사용하도록 학습한다. (feature identity를 강요하는 대신 공유 기억 기저를 비슷한 방식으로 사용하도록 유도한다)(RNA 토큰 쪽에는 stop-gradient를 걸어서, 정답이 학생 쪽 손실에 끌려 움직이지 않게 고정한다. 메모리 기저들이 한쪽으로 몰리거나 서로 겹치지 않도록, vector quantization 방식의 commitment 항과 기저 간 직교성 항으로 메모리를 정규화한다.)

학습이 끝나면 rna branch와 증류 손실 항을 제거한다. 그러면 최종적으로는 50 expert가 학습된 wsi branch만 모델에 남는다. 이 expert token을 gated aggregation으로 모아서 linear head에 넣으면 바이오마커 분류나 생존 예측에 사용할수있다. 
