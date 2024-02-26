---
제목 : SBERT 
날짜 : 2023 년 1 월 4 일 19시 10:00 -0400 
카테고리 : DL
---
Reimers, Nils, and Iryna Gurevych. "Sentence-bert: Sentence embeddings using siamese bert-networks." arXiv preprint arXiv:1908.10084 (2019).
## Motivation

SBERT는 컴퓨팅 리소스를 적게 먹어서 학교 프로젝트나 개인적인 공부 용도로 사용하기에 좋은 모델입니다. 

## BERT란?

SBERT를 이해하기 위해서는 먼저 BERT를 알아야 합니다. <br>
BERT는 Transformer의 Encoder를 쌓아서 만든 것으로 Base와 Large 버전이 있습니다.<br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/8a99ef72-181f-47dc-b94a-a309f213ebbb)
#### BERT Base , Large <br>
Encoder layer : 12, 24<br>
Embbeding vector size : 768, 1024 <br>Multi-head attention 12, 16 <br>Parameter 110M, 340M<br>

으로 GPT-3의 Parmeter가 175b 인 것에  비하면 base 기준 1/1000 사이즈로 개인이 훈련시킬 수 있을만한 크기입니다. <br>

#### BERT 훈련 방법

BERT는 Pre-trained 된 모델이고, 논문에 의하면 <br>

위키피디아 25억 단어 + Bookcorpus의 8억 단어 = 총 33억 단어를 가지고 train을 시켰다고 합니다. <br>

Base 기준 input 값으로 단어 시퀀스 최대 512개 이며, 시퀀스 하나하나의 길이는 768 크기의 벡터입니다.  <br>

12개의 Encoder를 Pre-train 시켜서 33억개의 단어로 크게 두가지의 형태로 훈련을 했습니다.

#### Masked Language Model, MLM

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/a07f5eaa-6972-4545-aa5f-25b187193bf1)

첫 번째로 입력 문장에 Mask 라는 빈칸을 추가하여 출력으로 빈칸 Mask 가 나오게 하는 학습이 있습니다. <br>

#### Next Sentence Prediction, NSP

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/a0adadf7-ffd5-4bb4-ac36-2d051a8ae75b)

두 번째로 NSP란 클래스를 뜻하는 스페셜 토큰 CLS, 문장의 사이를 분리하여 주는 SEP 토큰을 활용하는 형태입니다. <br>
두 문장이 서로 이어지면 Label = IsNextSentence True, 아닌 경우 NotNextSentence 를 출력 하게 학습시킵니다. <br>
추가로 거짓 단어를 집어 놓고 진짜 단어 인지 아닌지 학습하도록 시키기도 하였습니다. (king, play)<br>

#### Embeddings

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/3cd0f718-cd02-4ff9-a6a4-2c9bc6c6a770)

단순히 단어만 입력하는 것이 아니라, Position 임베딩, Segment 임베딩도 더해서 같이 입력합니다. <br>

Position 임베딩은 sin, cosine 섞어서 사용하고, <br> 첫번째 문장은 0, 두번째 문장은 1로 세그먼트 임베딩을 하여서 각각의 벡터를 더하면, <br> 768 + 768 + 768  = 768 (벡터)로 나오게 됩니다.

#### Fine-tuning

앞에서도 말씀 드렷듯이, BERT는 단순히 Pre-trained 된 모델 입니다. <br> BERT를 가지고 작업을 하려면 목적에 맞게 파인 튜닝이 필요합니다. <br> 논문에서는 크게 4가지의 Task에 맞춰 Fine-tuning 을 진행하였습니다. <br> 

#### 1. Single Text Classification

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/ba092b8e-356a-4edb-99dd-1fdb86ff6482)

긍정적인 문장을 집어 넣으면 아웃풋 1개 노드에서 Positive가 나오게 Tunning 하는 것이 Single Text Classification 입니다. <br>

#### 2. Tagging

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/6365fc95-731b-400e-8cce-a7f0877255f2)

Tagging이란 문장의 단어의 성분이 무엇인지 파악하는 Task 입니다. <br> 
Tag의 노드 갯수 많큼 dense layer를 붙여서 돌려 보냅니다. <br>

#### 3. Text Pair Classification or Regression

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/9d7200ea-ec13-4f24-948f-1ecb50c9c624)

대표적인 태스크로 자연어 추론(Natural language inference) : 두 문장이 주어졌을 때, 하나의 문장이 다른 문장과 논리적으로 어떤 관계에 있는지를 분류하는 것입니다. <br> 유형으로는 모순 관계(contradiction), 수반/암시 관계(entailment), 중립 관계(neutral)가 있습니다. <br> Regression을 붙이면 유사도를 어느 정도 알 수 있다고 합니다. <br>

#### 4. Question Answering

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/c0256caf-8002-4ce8-bf8d-235e17270848)

왼쪽에는 질문, 오른쪽에는 컨텍스트(질문에 대한 내용, 참조할 내용) 을 집어 넣고 각각의 Dense 를 통과하여 나온 Label 을 가지고 답을 유추해 나가는 방식입니다. <br>
Dense 라벨은 답과 관련 되어 있는 단어의 시작과 끝 포인트를 찾게 되고 답변과 관련이 없으면 패딩처럼 빈칸으로 나옵니다. 

## SBERT 

SBERT 란 BERT 의 여러 가지 Task 중, 3. Text Pair Classification or Regression 의 비효율적인 부분을 해결하기 위해 나온 모델입니다. <br>

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/9cb72ac6-a87f-4599-adcd-4e7cb3fe296b)
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/82e3fc86-28f6-4f26-a594-97bc88c68b22)


만 개의 문장으로 Task 를 수행하면, nC2 조합 경우의 수인 대략 오천만번의 inference를 수행하여야 합니다. <br>

v100 으로 자그마치 65시간이 소요 되었다고 합니다. <br> 그리하여 연구진들은 BERT에 1개의 문장만 집어 넣기로 하였습니다. 

#### Sentence Vector form Bert

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/3bb1bfab-01cd-479b-b2f0-e613bf2172f0)

버트를 사용해서 센텐스 벡터를 얻는 방법에는 두 가지가 있습니다. <br>

1. CLS 토큰을 잘 훈련 시켜 문장을 대변하는 벡터로 나타나게 하자 <br>
2. 출력 토큰들을 Average Pooling 으로 벡터 1개가 나오게 하자 <br>

이 중 2번이 효과가 좋았고, 2번이 바로 SBERT 의 아이디어가 됩니다.

#### Dataset

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/d3804b1e-8fd4-42d2-ace7-1d081c4e2a41)

NLI : 앞에서 주어진 두 개의 문장들의 관계(수반, 모순, 중립)를 맞추는 문제 <br>

SNLI(Stanford Natural Language Inference) - 570K 와 <br>
MNLI(Multi-Genre Natural Language Inference) - 433k 를 합쳐서 구성하였다고 합니다.<br>

#### Siamese Network

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/dbe5e472-a96d-4ec5-8875-89b58e8f6bbd)

샴쌍둥이를 뜻하는 이름에 걸맞게 동일한 BERT를 두 개 사용합니다. <br>

512개의 시퀀스가 들어가서 768 개의 벡터, 512by768 의 시퀀스를 AvgPooling 하면 768 벡터 1개가 나오게 됩니다. <br>

동일한 형태의 u, v, |u-v| 를 콘캣합니다. <br> 이걸 덴스레이어에 집어 넣고, Output 3개 짜리 Softmax 에 넣으면 NLI Task 의 3가지 분류로 구별합니다.  <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/d71b8774-b6e9-4ee2-af97-f4cfba905cf5) <br>

좀 더 자세히 살펴보면, 합친 NLI 900k dataset 로 sentence a, b 를 입력합니다. <br>

좌측 BERT 에서 Pooling 된 u 벡터, 우측 BERT 에서 Pooling 된 v 벡터, <br>
|u-v| 3 x 768 크기의 벡터를 콘캣하여 하나의 벡터로 만듭니다. <br> 이걸 Softmax 로 Label 과 비교하면서 계속 훈련을 합니다. <br>

#### pooling

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/8a105f37-aed9-41d1-bdb4-2fcd4bcd615b)

파란색 블럭들은 13개의 단어를 가진 문장의 토큰들입니다.
<br> 출력이 13x768 토큰이고 이를 하나로 만들기 위하여 평균도 내보고, 맥시멈도 해보고, 맥스 토큰도 써보았는데, 이 중에서 Mean-Pooling 이 제일 효과가 좋았다고 합니다. <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/7e40c746-6641-431a-abe1-18bb1d82db3c)<br> SBERT-NLI Table.

#### STS(Sementic Textual Similarity)
STS 란 두 개의 문장으로부터 의미적 유사성을 구하는 문제입니다.<br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/41e794e7-5249-4273-903b-8f2700f9e045)
<br> 여기서 Label 은 두 문장의 유사도로 범위는 0~5 입니다. <br>
레이블 값이 클수록 두 문장이 유사하다는 뜻입니다. <br>

#### STS Structure

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/18e89f5b-db9e-4d88-8bf3-88daed80a4de)

NLI 처럼 3가지 분류가 아니라 어떤 숫자 값으로 나오기 때문에 샤이아민 구조를 쓰는 것은 똑같지만 STS 에서는 u, v 벡터간의 Cosine similarity 를 구합니다. <br>
쉽게 풀어서 설명하면, u 와 v 를 dot product 한 다음에 길이로 나눠줌으로써 코사인 유사도 값을 구합니다. <br> 시밀러 값이 0~5 로 나오지는 않기 때문에, 값을 스케일링을 해줘야 합니다. <br>
 
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/b7f3e46f-7fe9-4dea-acea-9c14ee668e3a)

STS 튜닝만 진행하여도 NLI 튜닝보다 성능이 좋게 나왔으며, NLI + STS 튜닝을 하였더니 훨씬 더 성능이 좋게 나왔다고 합니다. <br>

## MS MACRO(Microsoft Machine Reading Comprehension)

연구진들은 90퍼센트에 달하는 성능에도 안주하지 않고 MS MACRO 데이터 셋에도 도전하였습니다. <br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/5d7efa1a-4b1a-4663-be89-bfc5f2f2622a) <br>

이 데이터 셋은 질문, 질문과 관련 있는 Positive, 관련 없는 Negative 값을 묶어둔 데이터 셋 입니다. <br>
 <br>

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/0b12113d-7d42-4076-8993-f6c87b0507d0)

이 번에는 anchor(문장), positive 문장, negative 문장을 각각 집어 넣습니다. <br>
나온 3개의 벡터를 트리플렛 로스로 계산합니다. <br>
anchor 벡터와 Positive 벡터의 차이, anchor 벡터와 Negative 벡터의 차이를 계산합니다.<br>
![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/0f5d31f4-ce90-4493-8801-9c6091c91bf6) <br>
(위키피디아 데이터셋은 문장, 문장과 가까운 Positive, 멀리 있는 Negative 이 세 값을 묶어둔 데이터입니다.)  <br> 그 결과, WiKiSec 에서 0.8078의 정확도를 달성하였다고 합니다. <br>

#### 부록 

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/953a0730-84fb-4661-880f-ab56010c842e)

이 표는 실험 결과를 정리한 표 입니다.<br>
NLI, STSb Task 에서 모두 Pooling 전략으로 평균 풀링이 가장 효과가 좋았고, <br>
NLI Task 에서 콘켓으로는 (u, v, |u-v|) 가 가장 좋았다고 합니다. <br>

#### MultiLingual SBERT 

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/d0e8d2a2-19cc-4d2a-abd7-e233d00b1292)

 Knowledge SBERT 모델을 Teacher Model 로 사용,<br>
Unknowlege SBERT 모델을 Student Model 로 사용합니다. <br>

두 모델에 동일하게 원어를 입력으로 하여, Student Model 에서 나온 벡터와 Teacher 모델에서 나온 벡터를 MSE Loss 를 사용하여 같아지도록 하고, <br> Dutch 입력을 Student Model 을 통과해 나온 벡터와 티쳐 모델의 벡터들 다시 MSE Loss 로 같아지게 훈련을 합니다.  <br> 이렇게 하여 다른 나라의 언어도 이해하는 MultiLingual SBERT 를 만들었다고 합니다. 

#### QA with Retrieval Augmented Generation (RAG)

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/aa0ac739-12a8-4aa1-a8bb-2f67070ec9ad)

졸업 프로젝트로 LLM 을 만든다고 할때,
LLM 의 동작 과정에서 Query 가 질의를 하면 LLM 이 모르기 때문에, <br>
외부 벡터 DB 에서 가져와야 합니다. <br>
여기서 임베딩을 SBERT를 통해 진행 할 수 있습니다. (컴퓨팅 리소스가 적게 들기 때문)<br>
쉽게 말하면, 사용자의 질문을 sbert 로 임베딩을 해서 llm 서버에 sbert 로 임베딩 되어 저장된 vector db 에서 검색을 한 후 Top-K 로 참조하여 질문을 LLM 에 넘겨주게 되면 LLM 이 답변을 하는 방식 입니다. <br>

#### HNSW (Hierarchical Navigable Small Worlds)

![image](https://github.com/ShinHyun-soo/ShinHyun-soo.github.io/assets/69250097/ed4d8a9c-24ed-4e41-917b-b5e36f3a5ad7)

사실 벡터 하나와 유사한 벡터를 백만개에서 찾기 위해 K-nn 을 쓰는 것은 미련한 방법이라고 합니다. <br> HNSW 를 사용하면 K 번만에 찾을 수 있는 파워풀한 알고리즘 입니다. <br>

Facebook의 함께 아는 친구는 3.5 다리만 거치면 다 연결되는 small world 이론을 이용하여 만든 알고리즘이라고 합니다. 

#### 추신(BERT 의 한계)

BERT 에서 파생된 것이 많은데, 핵심은 embbeding Pooling 기법입니다.<br>
하나의 Learnable Cls 를 쓸거냐 Average Pooling 을 쓸거냐. <br>
요새는 Transformer 든 Video 에서든 아무거나 써도 크게 차이는 없다고 합니다. <br> 
simease 구조는 contrastive 러닝, 즉 셀프 슈퍼바이즈드 러닝.<br>
모든 필드를 통틀어서 마스크드 기법이 좋다고 합니다. <br>
전체적인 Backbone 에서 인코더는 마스킹 기법이 좋다. <br>
Vision(비전 담당 교수님)쪽에서도 그렇다.<br>
이 분야를 Metric Learning 이라고 하는데, 결국 해보니 Masked 가 좋았다.<br>
근데 BERT 는 Masked 를 전체 연산에서 15% 만 한다. <br>
Masked autoencoder 은 Masked 를 75% 한다. <br>
2022, 2023 최고 키워드가 Masked 이다.<br>
그럼에도 BERT 를 쓰는 것은 기업이 아니고선 GPT 를 학습시킬 컴퓨터가 없다.<br>
보통 gpt api 를 사용하고 실력이 조금 되면 llamma 2 나 위스트랄 , 한국에서 나온 솔라를 파인 튜닝하여 사용합니다. <br>
컴퓨팅 리소스가 충분한 기업 같은 곳에서는 오픈 llm 을 사용함.<br>
근데 기업에서 강의를 오픈을 안함, 부스트 캠프 아니고선 요즘 nlp 를 공부하기 힘들다. <br>
유튜브에 SBERT 이용 강의가 많은 것도 학습자들 컴퓨터에서 돌릴 수 있는게 SBERT 가 밖에 없어서 그렇다. 
<br>
요즘 diffusion DPO 가 핫하다. <br> dpo 는 랭킹 로스학습 강화학습 같은 건데 강화학습은 아님.

