# CHAP 04_2

## Artificial Neural Networks (ANN)
* 매우 강력한 classification 모델
* 복잡하고 nonlinear decision boundary도 표현 가능 
* 이미지 분류, speech, 언어 처리 같은거 가능 -> 광범위하게 적용될 수 있음 
* 다른 classification 모델의 성능을 능가함 (사람도 능가)
* 인간의 뇌를 모방하려는 시도로부터 시작됨 

## Human Brain Model
* 서로 연결되어있는 수많은 노드들로 구성됨 
    * `Nodes`: `뉴런`. 계산의 기본적인 단위
    * `directed links`: 뉴런 사이들의 연결 
    * the `weight` of a directed link: 뉴런들 사이 연결의 강도 
* ANN의 주요 목적: `input-output의 관계`를 잘 나타낼 수 있는 `weight(가중치)`를 찾아내는 것 

## Basic Motivation behind a ANN model
* 서로 연결된 **노드**들의 **복잡한 조합**을 사용함으로써 ANN 모델들은 더 풍부한 **feature**들을 자동적으로 추출해낼 수 있음 (PCA와 같은 traditional hand-crafted feature extraction과 달리..)
* 다수의 level들에서 feature들을 나타낼 수 있는 자연스러운 방법을 제공 
    * `복잡한` feature들은 `단순한` feature들의 조합으로 나타내어짐 
    * **low level feature에서 high level feature를 알아낼 수 있음** 

## ANN의 발전과정
1. `퍼셉트론 (Perceptron)`: 가장 단순한 ANN 모델 
    * input 노드와 output 노드가 존재 
    * 그림 ppt 참고 
2. `Multi-layer neural networks`: 더 복잡한 구조 
    * 그러나 **학습이 잘 안되고 계산량이 너무 많음** 
3. `Deep learning` 
    * 더 많은 연구가 되어 더 deep 한 구조와 함께 현대의 ANN 모델들이 효과적으로 학습할 수 있게됨
    * 인기가 많아짐 

## 뉴런이 하나만 있을 떄의 한계 
* `퍼셉트론`이나 `logistic regression`이 하나만 있는 것은 오직 `linear한 decision boundary`만 학습 가능함 
    * 왜냐하면 decision들은 w1x1+..+wnxn+b>0에 근거하고 있기 때문 
* 그러나 현실 세계의 분류 문제들은 **`nonlinearly separable classes`**들을 가지고 있음 
* multi layer가 필요하다! (ex. XOR 연산같은 경우 linear boundary로 표현이 안됨-> multi-layer가 필요)

## Multi-layer Neural Network
* 뉴런의 기본적인 개념을 더 복잡한 노드의 구조들로 일반화시킴 
    * `layers`: 연관된 노드들이 그룹들로 존재하는 것
    * 더 `복잡`하고 `nonlinear`한 decision boundary들을 학습 가능 
* 구성 
    1. `Input Layer`: input들을 나타냄
    2. `Hidden Layers`: 전의 layer로부터 `signal`들을 받음, 다음 layer로 `activation value`들을 전달함
    3. `Output Layer`: output variable들을 예측값을 만들어냄 
* Multi-layer Neural Network의 장점 
    * `mutliple`한 hidden layer들을 가지고 있으므로 `많은`, `복잡한` `hyperplane`들을 만들어낼 수 있음 
        * output node는 단순히 hidden node들의 결과들을 결합함으로써 decision boundary를 산출해낼 수 있음 
        * 퍼셉트론이나 logistic regression의 경우 hidden layer가 존재하지 않아 오직 **하나**의 hyperplane만을 만들어낼 수 있음 (그러나, XOR problem을 해결할 수 없음)

## Hidden Layers in ANN
* classification에 유용한 **`잠재적인 feature들`**을 학습하는 것이라 봐도 됨 
    * `Layer 1`: x1,x2,..xp로부터 단순한 feature들을 잡아냄
    * `Layer 2~L-1`: 더 높은 수준의 featuer들로 값들을 결합하여 만들어냄
    * `Layer L`: **가장 높은 수준**의 feature들을 결합하여 예측값을 도출 
* ANN은 매우 표현을 잘해낼 수 있음 
    * 더 **`복잡한 feature`**들을 얻어낼 수가 있음 

## ANN의 노드들에서의 계산
* PPT 참고 
* `activate function`들의 종류 : `real-valued` 그리고 `nonlinear`한 value들을 만들어낼 수 있음 
    * ReLU function: f(z)=max(0,z) -> 딥러닝에서 사용하는 function
    * **`Sigmold function`**: f(z)= 1/(1+e^-z) -> 많이 사용됨 
        * **딥러닝에서는 사용하기 어려운 모델** (이유: *weight를 증가시켰을 때 weight가 어떻게 변할지를 예측하기 어려움.* 값이 증가할수록 수평이라 판단이 어렵기 때문. 대신에 ReLU function을 사용함)
    * Tanh function: -1~1 사이의 값 (작아질 수록 -1로 값을 보냄) -> 잘 안 쓰임 

## model parameter 학습 
* w: 모든 weight들의 값 (large scale vector)
* b: 모든 bias들의 값 (large scale vector)
* **`cost(loss) function`** 정의 필요됨 
    * 모든 instance들을 각각 error들의 합 => `error들의 총량` 
    * J(w,b)를 `최소화`하는 w와 b를 찾아야함 
        * **미분이 어려움** -> `gradient descen`t 사용 -> 그러나 이것도 locally optimal한 solution임 -> 여전히 `hidden layer`에서 계산이 아주 어려움 (이유: w는 직접적으로 y추정값에 영향을 주지 않지만, 복잡하게 얽힌 영향들이 연속적인 layer들의 activation value를 가지고 있기 때문)
        * `Backpropagation` 필요! 