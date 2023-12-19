---
제목 : Linear Regression
날짜 : 2023 년 12 월 19 일 21시 10:00 -0400 
카테고리 : DL
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Linear Regression

&nbsp;인공지능에서 선형 회귀를 사용하면 기존의 데이터 셋을 바탕으로 새로운 데이터를 예측 할 수 있습니다. <br> 이 글에서는 예측 과정을 수학적으로 분석하여 원리를 탐구해 보도록 하겠습니다.

## 목차

- 선형 회귀 수식
- 로스 함수
- 업데이트 식
- 편미분
- 최종 업데이트 식
- 데이터 셋
- 실제 계산

## 선형 회귀 수식
$$\hat{y}= \theta_1 x + \theta_0 $$ <br>
&nbsp;이 수식이 바로 학습 전 초기 모델입니다.  
세타는 학습 가능한 파라미터로 학습을 통해 업데이트 됩니다.  


## 로스 함수
&nbsp;뿐만 아니라 로스 함수도 필요합니다.  
로스 함수는 MSE Loss 를 사용 하도록 하겠습니다.  
$$ L = (y -\hat{y})^2 $$ <br> 
MSE Loss 란 평균 제곱 오차를 뜻하며, MSE Loss 를 사용 하는 이유에 대한 해석은 크게 2가지로, Maximum likely hood와 Convex 하게 하기 위함이 있습니다.   
MLE 는 확률 분포를 맞추는 수식을 전개하다 보면 MSE 함수가 나오게 되는데 정말 신기합니다.  
Convex 하게 하기 위함이란 Loss 함수를 최적화 하는데 용이하게, 쉽게 말하면 로스 함수를 미분 가능하게 만들고자 오차 식에 제곱을 하는 것입니다.

## 업데이트 식
세타1과 세타0 에 대한 업데이트 식은 각각 다음과 같습니다. <br>
$$ \theta_1 =\theta_1 -lr \cdot  \frac{\partial L}{\partial \theta_1}  $$  
$$ \theta_0 =\theta_0 -lr \cdot  \frac{\partial L}{\partial \theta_0}  $$

&nbsp; 여기서 lr 은 학습률로 업데이트 과정에서 일어나는 문제를 예방하기 위한 감쇠 인자 입니다.  
학습률로 조절하지 않는다면 세타가 너무 작아지거나 커질 수 있습니다.

## 편미분
로스 함수에 모델을 대입하면,  <br>
$$ L(\theta_1, \theta_0) = (y-(\theta_1 x + \theta_0))^2 $$  <br>
$$ = x^2(\theta_1)^2+(\theta_0)^2-2xy\theta_1-2y\theta_0+2x\theta_1\theta_0+y^2$$ <br>
이고, 로스를 각 세타로 미분하면,  <br>
$$ \frac{\partial L}{\partial \theta_1} = -2x(y-\theta_1x-\theta_0) $$
$$ \frac{\partial L}{\partial \theta_0} = -2(y-\theta_1x-\theta_0) $$
입니다. 

## 최종 업데이트식
업데이트 식에 편미분 값을 합치면 다음과 같이 나오게 됩니다.  <br>
$$\theta_1 = \theta_1 + 2\alpha x(y-\theta_1x-\theta_0) $$
$$\theta_0 = \theta_0 + 2\alpha (y-\theta_1x-\theta_0) $$

## 데이터 셋

&nbsp; 세타를 업데이트 하기 위해서는 데이터 셋이 필요합니다.  
학습이 이뤄지는 것을 보기 위해 y = 3x + 2 에서 데이터 셋을 만들도록 하겠습니다.
$$ Dataset = (1,5), (2,8),(3,11) $$

## 실제 계산
&nbsp; 이제 드디어 업데이트 식에 데이터 셋을 대입해 계산해 보도록 하겠습니다. (초기 세타 (1, 1), 학습률 0.1 ) <br>
$$\theta_1=1+0.2(5-1-1) =1.6 $$ <br>
$$\theta_1=1.6+0.4(8-3.2-1) = 3.12 $$ <br>
$$\theta_1=3.12+0.6(11-9.36-1) = 3.504 $$ <br>
$$-$$ <br>
$$\theta_0=1+0.2(5-1-1) =1.6 $$ <br>
$$\theta_0=1.6+0.2(8-2-1.6) = 2.28 $$ <br>
$$\theta_0=2.28+0.2(11-3-2.28) = 3.424 $$ <br>
3 iteration 을 통해 구한 모델은 다음과 같습니다. <br>
$$ \hat{y} = 3.504x+3.424 $$ 
 
데이터 셋을 추가하여 정답 값인 3x+2 에 도달하는지 직접 해보셔도 좋을 것 같습니다.   

### 출처
&nbsp; 신경식 강사님의 '수학적으로 접근하는 딥러닝' 이란 강의로 공부한 내용입니다.