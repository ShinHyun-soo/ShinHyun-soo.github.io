---
제목 : Mobile Diffusion Study 
날짜 : 2024 년 2 월 16 일 19시 10:00 -0400 
카테고리 : DL
---

MobileDiffusion: Subsecond Text-to-Image Generation on Mobile Devices [Zhao, Yang, et al., arXiv 2023]    

요약 : MobileDiffusion은 아키텍쳐와 샘플링 기술 최적화를 통해 모바일 기기에서 512 x 512 이미지를 1초 미만의 시간으로 생성할 수 있게 함.    

최적화 기법 :   
1.  
   고해상도 트랜스포머를 저해상도(_fewer layers_) 트랜스포머로 대치.     
   저해상도 트랜스포머의 채널 크기(_Using Separable Conv_)를 줄임.
   이렇게 해도 정량적 측정 기준이나 시각적 품질에 영향 없었음.
   하지만, 트랜스포머의 폭(_input dim_)을 줄이는 것은 시각적 품질에 악영향을 끼쳤음.
   따라서, 더 낮은 해상도의 더 많은 트랜스포머 블럭들을 채널 수를 줄여 통합하였음. - (품질 저하 없이 런타임 효율 26% 향상)    
   &nbsp;
2.   
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/3dfec663-b9bd-4ba6-ae9d-95eb2b7b2e6c)    
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/226dc21d-70e0-4b75-91da-113e24fad157)
   셀프 어텐션은 장거리 종속성(*long-range dependencies*)을 캡쳐하는데 중요한 역할을 하지만, 특히 고해상도에서 많은 계산량을 요구함.    
   기존 연구에서는 전체 트랜스포머 블럭들을 낮은 해상도로 재배치 함으로써 고해상도에서의 self-attention 과 cross-attention 을 제거함.    
   위와 같이하면 성능이 눈에 띄게 저하됨. (FID`Frechet Inception Distance` : 12.50 -> 17.87, CLIP score : 0.325 -> 0.302)
   고 해상도에서 셀프 어텐션 레이어들만 제거하면 성능 저하 없었음.
   text guidance 가 이미지의 글로벌 레이아웃과 로컬 텍스쳐에 중요하기 때문에 크로스 어텐션(이하 CA)이 다양한 해상도에서 중요하다고 추측함.
   특히, 텍스트 임베딩의 시퀀스가 더 짧기 때문에, 고해상도에서 CA의 계산 비용이 SA 보다 훨씬 낮음.
   따라서 SA 만 제거하면 효율성이 크게 향상됨.
   최고 해상도(64x64)에서 트랜스포머 블럭(SA,CA)을 완전히 제거.
   32x32와 외부 16x16에서 SA 제거.
   내부 16x16과 가장 안쪽 병목 스택은 완전환 트랜스포머 블럭 유지. - (품질 저하 없이 런타임 효율 15% 향상)
   &nbsp;

3. 키와 벨류 projections 공유. 
   K = x · Wk , V = x · Wv
   여기서 Wk 와 Wv 를 공유해도 모델 성능에는 부정적인 영향을 미치지 않았음. - (parameter count 5% 감소.)