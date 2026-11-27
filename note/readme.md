# 논문 리뷰

1. A Pathway-Structured Privileged Distillation Framework for Deployable Computational Pathology [#](https://github.com/younsoid/works/blob/main/note/paper1.md)

# 결과 정리

### 1. 데이터 준비

1-1. 코호트 구성 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note12.md)

1-2. MFP 29 signature 재현 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note13.md)

1-3. MFP subtype 재현 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note13.md)

1-4. MFP score 보조 데이터 [#](https://github.com/younsoid/works/blob/main/note/note1.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note13.md)

1-5. 보조 오믹스 [#](https://github.com/younsoid/works/blob/main/note/note2.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note14.md)

1-6. 보조 오믹스별 정보량 [#](https://github.com/younsoid/works/blob/main/note/note2.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note16.md)

### 2. H&E slide 처리

### 3. Slide tiling과 QC

3-1. 타일링

3-2. 품질 기준

3-3. 통과율

### 4. Exaonepath feature extraction

4-1. 특징 추출

4-2. 슬라이드별 요약

### 5. Hovernet 핵분할

5-1. 설정

5-2. 대상

5-3. 전체 실행

### 6. Head design과 보조오믹스

6-0. Fold design

6-1. Head design  [#](https://github.com/younsoid/works/blob/main/note/note3.md)

6-2. G2 확인

6-3. G6 확인

6-4. G7 확인



 
6-8. 보조 오믹스의 설명력

6-10. 29 signature의 보조 오믹스 채택

6-11 Head design별 보조 오믹스 채택

### 7. 모델과 학습 설계

7-1. 모델 설계 [#](https://github.com/younsoid/works/blob/main/note/note17.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note18.md)

7-2. 학습 설계 [#](https://github.com/younsoid/works/blob/main/note/note17.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note18.md)

7-3 기준선: 슬라이드 평균 특징 + 릿지 [#](https://github.com/younsoid/works/blob/main/note/note19.md) / update: [#](https://github.com/younsoid/works/blob/main/note/note20.md)
 
7-4. 실행 [#](https://github.com/younsoid/works/blob/main/note/note19.md)

7-5. 학습 양상

### 8. G0 attention 및 G7

8-1. G0 attention 확인 [#](https://github.com/younsoid/works/blob/main/note/note8.md)

8-2. Attention similarity matrix의 일관성 [#](https://github.com/younsoid/works/blob/main/note/note8.md)

8-3. G6과 G7 비교 [#](https://github.com/younsoid/works/blob/main/note/note8.md)

8-4. Attention similarity와 RNA coexpression [#](https://github.com/younsoid/works/blob/main/note/note10.md)

8-5. Attention 집중도와 학습 길이

### 9. 성능 평가

9-1. 성능 지표 [#](https://github.com/younsoid/works/blob/main/note/note21.md)

9-2. 판정 [#](https://github.com/younsoid/works/blob/main/note/note21.md)

9-3. 기본 성능

9-4. seed 평균의 효과

9-5. 기준선 대 단일 모델

9-6. LUAD 묶기 효과

9-7. LUSC 묶기 효과

9-8. Δr 분해

9-9. 그룹 내용 효과

9-10. 묶이면 손해 보는 시그니처

9-11. 기능 그룹 층화

9-12. 보조 오믹스 효과

9-13. 아형 재현

9-14. 공발현 정합도와 이득

9-15. 강건성·독립 지표·기관 신호

9-16. 학습 자료 크기 곡선

### 10. Attention 검증

10-1. 원칙

10-2. 같은 seed 효과

10-3. 묶기가 attention을 안정시킴

10-4. 보정 일치도로 본 전체 변화

10-5. G2 헤드 겹침

10-6. attention 변화와 Δr

10-7. Head design 효과 확인

10-8. 헤드 크기 규칙

10-9. 짝 분석

10-10. 보조 오믹스 효과 확인

10-11. 민감도: 고정 epoch, 가산형 기여도에서도 결론이 유지되는지 (진행 중)

### 11. 바뀐 칸과 슬라이드 선정

11-1. 슬라이드 단위 변화량

11-2. 변화량과 MFP 아형

11-3. 대상 칸

### 12. 핵 분할 타일 집합 

12-1. 집합 정의

12-2. 상위 슬라이드는 타일 단위에서도 다른 곳을 봄

12-3. 규모


### 13. 공간 표현형 (TODO)

13-1. 타일 구획

13-2. 표현형 판정

13-3. 판정 검증

13-4. MFP 아형 × 표현형

### 14. 바뀐 타일의 조직 특성 (TODO)

14-1. 바뀐 타일의 세포 구성

14-2. 대상별 해석

14-3. 헤드 구획 비중 × 공간 표현형

14-4. 같은 아형 안 표현형 간 오믹스 차이

### cf

cf) 모델 기본 성능 [#](https://github.com/younsoid/works/blob/main/note/note6.md) / grouping 이득 확인 (luad) [#](https://github.com/younsoid/works/blob/main/note/note6.md) / grouping 이득 확인 (lusc) [#](https://github.com/younsoid/works/blob/main/note/note6.md)

cf2) 연구 배경 [#](https://github.com/younsoid/works/blob/main/note/note11.md)

cf3) attention head 그룹화 [#](https://github.com/younsoid/works/blob/main/note/note15.md)

cf4) G6 확인 [#](https://github.com/younsoid/works/blob/main/note/note3.md) / G6 k=2와 G1 [#](https://github.com/younsoid/works/blob/main/note/note4.md) / G6 k=4 [#](https://github.com/younsoid/works/blob/main/note/note4.md) / G6 k=4 (2) [#](https://github.com/younsoid/works/blob/main/note/note5.md) (3) [#](https://github.com/younsoid/works/blob/main/note/note9.md) / 각 Head design의 결속 측정 [#](https://github.com/younsoid/works/blob/main/note/note5.md) / G2 확인 [#](https://github.com/younsoid/works/blob/main/note/note5.md)
