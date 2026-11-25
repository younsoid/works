# 연구 배경


#1

<img width="500" alt="image" src="https://github.com/user-attachments/assets/f07964f7-246c-4fd4-9374-f58124643f2a" />

Bagaev 논문에서는 29 MFP signature를 정의하고, 그 signature를 기능에 따라 Anti-tumor / Pro-tumor / Angio·Fibrosis / Malignant로 나눴다. 이 Bagaev의 미세환경 signature 분류는 미세환경 연구의 기능적 분류에 많이 사용되고 있다. 

해당 논문은 이 29 signature의 expression pattern을 사용해서 환자를 IE·IE/F·F·D type으로 분류했다. (TCGA 8,024명 환자간 Pearson correlation -> Louvain clustering) 그리고 4개 type은 immune inflamed, immune excluded, immune desert 라는 특성의 연속 축 위에 있다고 말한다. 

#2 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ef7ff1b7-f904-4c46-b9c6-66c30fc3340c" />

HistoTME 논문은 WSI를 받아서 이 29 signature를 예측하는 연구이다. 이때 attention head를 Bagaev functional group인 Anti-tumor / Pro-tumor / Angio·Fibrosis / Malignant로 grouping 해서 multi task training을 수행했다. 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/dc1110b3-9409-49ca-9503-22980a35913b" />

수행 결과 예측 성능은 grouping 전과 비교했을때 비슷했다. 그리고 그림처럼 Anti-tumor / Pro-tumor group의 attention 을 시각화해서 비교했을때 유사한것을 확인했다. 

즉 기능적으로 완전히 반대인 signature라 하더라도 같은 부분에 attention을 줄 수 있다. 그렇다면 기능적으로 유사한 signature라 하더라도 다른 부분에 attention을 줄수 있다. 기능적 grouping도 좋지만 모델이 signature를 wsi에서 잘 학습하게 하여 wsi에서 생물학적으로 유효한 근거를 찾으려면, head grouping에 대해 새로운 방법이 필요하다. 

#3 

WSI로 RNA를 예측하려는 연구들에서는 RNA 예측 성능을 놓고 개선 한계를 언급하기도 한다. WSI로 예측 성능이 낮은 RNA signature 중에는 WSI 이미지에서 잘 드러나지 않는 신호가 있을 수 있다는 것이다. 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/be5161a5-19f0-4c2c-ac40-d39314e76aa5" />

실제로 HE2RNA 연구에서 WSI를 사용해서 예측이 잘 되는 RNA 경로와 상대적으로 그렇지 않은 RNA 경로가 있었다. (면역 경로(T 세포 86%, B 세포 75%)는 대부분의 암종에서 무작위보다 잘 예측, 나머지 네 경로(혈관신생 50%, 저산소·DNA 복구·세포 주기 54%)는 절반 정도의 암종에서만 예측)

<img width="500" alt="image" src="https://github.com/user-attachments/assets/f83a8d3b-9bcf-4fd5-9bee-f50b70bf34bc" />

Ash 연구는 유전자 발현과 이미지 특징을 함께 분석(CCA)해서 "형태와 같이 변하는 유전자 묶음"을 식별하였는데 세포외기질, 혈관, 혈액·면역 세포 구성처럼 세포 구성이나 조직 구조로 보이는 유전자 묶음이 형태와 연결됨을 확인했다. 즉 WSI 형태로 식별이 용이한 경로가 있고 그렇지 않은 경로가 있음을 알수있다.

#4

RNA 예측 성능을 높이는 방법으로 타 오믹스를 함께 학습하는것이 있다. RNA와 함께 타 오믹스를 학습해서 성능을 올린 연구도 있고 최근에는 RNA 없이 타 오믹스만으로 RNA를 예측하기도 했다. (Estimating gene expression from DNA methylation and copy number variation) 

<img width="300" alt="image" src="https://github.com/user-attachments/assets/c33ed82a-e7b3-4975-86bd-6e83348c3b8a" />

HE2RNA 연구에서도 단백체와 RNA가 서로 담고있는 정보가 다르기 때문에 함께 사용하는것이 면역학적 분석에 유용하다고 언급하고 있다. 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/9fc5a25c-7f5c-49ad-8111-7e0376e8a0d7" />

실제로 WSI 예측 모델에서 다양한 방법으로 타 오믹스 정보를 반영하는 연구가 있다. HistoMethyl 연구에서는 사전학습 정렬로 이를 구현했고, 유전자 변이, 생존, 메틸화 β값 예측에서 성능 향상을 확인했다. 

이처럼 학습 단계에서 다른 오믹스를 참조하는 다양한 방법이 있는데 (HistoMethyl, MKD, G-HANet, PathLUPI, Fuji) 예측 대상이 RNA인 연구는 없었다. 이에 RNA 미세환경 시그니처를 주 정답, 타 오믹스를 보조 정답으로 한 예측 연구를 설계하게 되었다.

<img width="300" alt="image" src="https://github.com/user-attachments/assets/1c1a8c4e-31a5-4b99-a589-fa83b2550a70" />

미세환경 연구 분야에서는 Nahhas 연구가 타 오믹스 반영을 보조 정답으로 구현한바 있다. 하지만 분류 대상이 MSI·HRD 분류였으며 미세환경 지표를 보조 정답으로 사용하였다. 해당 연구에서 사용한 미세환경 지표는 DNA 메틸화 기반 백혈구 비율 (LF), 복제수 + 메틸화 기반 기질 비율 (SF), RNA 기반 림프구 침윤 점수 (LISS), RNA signature인 증식 (Prolif), 복제수 기반 종양 내 이질성 (ITH) 데이터였다. 

#5

따라서 연구의 전제는 다음과 같다.
1. 기능적으로 완전히 반대인 RNA signature라 하더라도 같은 부분에 attention을 줬을 수 있다. 기능적인 grouping이 아닌 새로운 grouping이 필요하다.
2. WSI로 예측 성능이 낮은 RNA signature 중에는 애초에 WSI에 잘 드러나지 않아서 였을수 있다. 이런 Signature들을 함께 학습시키면 부정적 전이만 일어나게 된다.
3. 학습 단계에서 모델이 WSI뿐만 아니라 다른 오믹스를 참조하게 하면, WSI에 잘 드러나지 않는 몇몇 rna signature들이 attention하는 부위가 해당 오믹스 측면에서도 관찰할수 있다. 즉 attention이 변화한 부위가 생물학적으로 그 signature의 특성이 해당 오믹스가 반영되었을때 비로소 보인 attention 부위 발굴일 수 있다.

연구 설계는
- 먼저 29 signature에서 rna 기능 group(기존 Bagaev 연구), rna coexpression group(폐암 특이적 group), co attention group(별도의 attention head를 할당했을때 29 signature 각각의 attention similarity로 grouping)을 식별하여 rna 기능이 유사하지만 attention이 다른 signature, rna 기능이 다르지만 attention이 유사한 signature를 찾는 연구를 설계하였다.
- 또한 각 group으로 head를 묶은 모델의 attention을 확인하여, 그 차이가 이미지의 어떤 부분에서 발생하는지를 보고 그 부분의 미세환경 특성에 대한 생물학 분석을 진행하는 연구를 설계하였다.
- 추가로 각 group으로 head를 묶을때 group마다 보조 정답 오믹스를 선택하여 모델을 학습시키고 마찬가지 attention의 차이를 확인하여 미세환경 분석을 진행하였다.

(기대 결과)
1. wsi 로 미세환경 rna를 예측할때 rna 가 coexpression 되는 group과 grouping 없이 단일 head로 할당했을때 attention이 coattention 되는 group을 찾고 두 group의 차이를 보면 RNA는 co expression 되지만 attention이 다른 경우를 식별할수있고 이건 의미가 있다 친종양 항종양이 같은 곳을 봤기도 하고. 그리고 두번째로 각 group으로 head를 묶어서 모델을 학습하고 attention이 차이나는곳을 hovernet 등으로 세포 수준으로 확인하면 그 차이를 세포 수준에서 확인 해볼수있다. 
2. wsi 로 미세환경 rna를 예측할때 보조 정답으로 오믹스를 넣어주면 rna로만 봤을때는 드러나지않던 해당 시그니처의 특이적 attention을 확인할수있고 마찬가지 보조 오믹스 없을때와 attention이 차이나는곳을 hovernet 등으로 세포 수준으로 확인하면 그 차이를 세포 수준에서 확인 해볼수있다. 

#출처
- Bagaev 논문 - Conserved pan-cancer microenvironment subtypes predict response to immunotherapy
- HistoTME 논문 - Predicting the tumor microenvironment composition and immunotherapy response in non-small cell lung cancer from digital histopathology images
- HE2RNA 논문 - A deep learning model to predict RNA-Seq expression of tumours from whole slide images
- Ash 논문 - Joint analysis of expression levels and histological images identifies genes associated with tissue morphology
- HistoMethyl 논문 - A multimodal foundation model linking histopathology and DNA methylation
- Nahhas 논문 - Joint multi-task learning improves weakly-supervised biomarker prediction in computational pathology

