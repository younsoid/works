# 연구 배경

<img width="500" alt="image" src="https://github.com/user-attachments/assets/a233d9b8-efbc-4528-a931-f790d6250f0b" />

Bagaev 논문은 Conserved pan-cancer microenvironment subtypes predict response to immunotherapy이다. 이 논문에서는 29 MFP signature를 정의하고, 그 signature를 기능에 따라 Anti-tumor / Pro-tumor / Angio·Fibrosis / Malignant로 나눴다. 이 Bagaev의 미세환경 signature 분류는 미세환경 연구의 기능적 분류에 많이 사용되고 있다. 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/a347feaa-1774-46e1-a514-03c99a976645" />

해당 논문은 이 29 signature의 expression pattern을 사용해서 환자를 IE·IE/F·F·D type으로 분류했다. (TCGA 8,024명 환자간 Pearson correlation -> Louvain clustering) 그리고 4개 type은 immune inflamed, immune excluded, immune desert 라는 특성의 연속 축 위에 있다고 말한다. 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ef7ff1b7-f904-4c46-b9c6-66c30fc3340c" />

HistoTME 논문은 WSI를 받아서 이 29 signature를 예측하는 연구이다. 이때 attention head를 Bagaev functional group인 Anti-tumor / Pro-tumor / Angio·Fibrosis / Malignant로 grouping 해서 multi task training을 수행했다. 

수행 결과 예측 성능은 grouping 전과 비교했을때 비슷했다. 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/dc1110b3-9409-49ca-9503-22980a35913b" />

그리고 Anti-tumor / Pro-tumor group의 attention 을 비교했을때 유사하다고 나왔다. 즉 기능적으로 완전히 반대인 signature라 하더라도 같은 부분에 attention을 줬을 수 있다. attention이 비슷한 group 끼리 head를 묶어야 부정적 전이가 발생하지 않기 때문에 head grouping에 대해 새로운 방법이 필요하다. 

###

WSI로 RNA를 예측하려는 연구들에서는 RNA 예측 성능을 놓고 개선 한계를 언급하기도 한다. WSI로 예측 성능이 낮은 RNA signature 중에는 WSI 이미지에서 잘 드러나지 않는 신호가 있을 수 있다는 것이다. 

실제로 HE2RNA 연구에서는 WSI에서 잘 드러나는 RNA 경로와 그렇지 않은 RNA 경로를 언급하기도 했다. (A deep learning model to predict RNA-Seq expression of tumours from whole slide images)

###

RNA 예측 성능을 높이는 방법으로 타 오믹스를 함께 학습하는것이 있다. RNA와 함께 타 오믹스를 학습해서 성능을 올린 연구도 있고 최근에는 RNA 없이 타 오믹스만으로 RNA를 예측하기도 했다. (Estimating gene expression from DNA methylation and copy number variation)

실제로 WSI 예측 모델에서 다양한 방법으로 타 오믹스 정보를 반영하는 연구가 있다. (HistoMethyl, MKD, G-HANet, PathLUPI, Fuji) 

HistoMethyl 연구에서는 사전학습 정렬로 이를 구현했고, 유전자 변이, 생존, 메틸화 β값 예측에서 성능 향상을 확인했다.

이처럼 학습 단계에서 다른 오믹스를 참조하고, 추론은 H&E만 쓰는 연구가 많지만 RNA 예측에 보조 정답으로 타 오믹스를 사용한 연구는 없다. 

###

따라서 연구의 전제는 다음과 같다.
1. 기능적으로 완전히 반대인 RNA signature라 하더라도 같은 부분에 attention을 줬을 수 있다. 기능적인 grouping이 아닌 새로운 grouping이 필요하다.
2. WSI로 예측 성능이 낮은 RNA signature 중에는 애초에 WSI에 잘 드러나지 않아서 였을수 있다. 이런 Signature들을 함께 학습시키면 부정적 전이만 일어나게 된다.
3. 학습 단계에서 모델이 WSI뿐만 아니라 다른 오믹스를 참조하게 하면, WSI에 잘 드러나지 않는 몇몇 rna signature들이 attention하는 부위가 해당 오믹스 측면에서도 관찰할수 있다. 즉 attention이 변화한 부위가 새로운 attention 부위 발굴일 수 있다.


