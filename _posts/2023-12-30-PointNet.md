---
제목 : Point Cloud Classification with PointNet
날짜 : 2023 년 12 월 30 일 18시 10:00 -0400 
카테고리 : DL
---

## Summary

기존의 ConvNet은 인풋으로 Ordered 한 데이터를 원함. <br>
하지만 3D 데이터는 Unordered 한 데이터임. <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/e3a9c288-e1f8-46d8-ba0c-78bb20bcb32c)
기존의 연구진들은 Point Cloud를 Voxel grid로 바꾸거나 2D Image Collection으로 Ordered하게 바꾸어 처리하려고 노력함. <br>
그럼 데이터의 양이 너무 커지는 문제점이 있음. <br>
그래서 Point Cloud 자체를 처리할 수 있는 신경망인 PointNet을 개발함. <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/611d6cbf-4777-4237-8df0-b624d7e325f6)<br>
순열 분별성을 위해 symmetric 한 연산자로 이루어진 MLP와 Max Polling Function 사용함. <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/15964871-7f4e-48e7-82a6-716c2f70d2ac)<br>
뿐만 아니라 점들이 회전해도 점들간의 거리나 방향이 그대로 유지되어야함. <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/9ce8f4f2-b565-4d6e-be97-26fc77beabf0) <br>
이를 위해 T-Net을 두 개 사용함. <br>
첫 번째 T-Net은 Spatial 변환을 수행. <br>
두 번째 T-Net은 Feature 변환을 수행. <br>
두 번째 T-Net은 차원이 훨씬 더 높기 때문에 최적화 하는데 어려움이 있음. <br> 
따라서 Orthogonal Regulizer로 두 번째 T-Net 을 직교 행렬에 가깝게 제한함. <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/e4e35f65-8089-4c67-94c6-3812c487a5e9)<br>
1. input 으로 (none, 2048(point), 3(xyz))을 받음.
2. T-net 으로 Affine 변환. (input transform)
3. MLP로 Feature 추출.
4. T-net 으로 Affine 변환. (feature transform)
5. Max Pooling Func 로 feature 극대화.
6. Dense layer 로 k class 후보에 대한 k score 값인 class label 을 output 으로 출력.


## Strength

순열 분별성을 지킨 것. <br>
Point Cloud 자체를 처리할 수 있는 네트워크 개발. <br>

## Weakness

PointCloud 변환 전 3D Mesh 데이터도 용량이 크고, 수집이 힘듦.

## Think Better Solution

2D 이미지 to Point Cloud 기술이 필요.
