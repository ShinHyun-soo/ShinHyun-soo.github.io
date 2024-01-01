---
제목 : Maximum Likelihood
날짜 : 2023 년 1 월 1 일 23시 10:00 -0400 
카테고리 : DL
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


### Classic Machine Learning

$$ \theta^* = arg\,\underset{\theta}{\text{min}}\,L(f_\theta(x),y) $$ 
<br>
클래식한 머신 러닝은 예측값과 차이를 줄이는 모델이다.

### Deep Neural Networks

DNN에서는 $$ L(f_\theta(x),y) = \sum_i L(f_\theta(x),y)$$ 이므로 Backpropagatio을 통해 DNN을 학습시키기 위해서는 <br>
1. DNN의 총 로스는 각각의 훈련 셋에 대한 로스의 합이어야 한다.
2. DNN의 최종 출력물은 각 훈련 셋의 로스여야 한다. 
<br> 와 같은 조건이 있습니다.

### Maximum Likelihood 관점

$$ \underset{\theta}{\text{max}}\,p(y|f_\theta(x)) $$

로스 대신 "정해진 확률분포에서 출력이 나올 확률을 최대화 한다." 로 보는게 최대우도법 관점입니다.

## Negative log-likelihood

각각의 random variable들이 독립적이고 같은 확률 분포를 가진다고 할 때, <br>

$$ p(y|f_\theta(x))
