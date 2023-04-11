# CHAP 04 

## Types of Classifiers
* binary vs. multiclass
    * 클래스가 둘 중에 하나를 선택하는 것: binary
    * 클래스가 두개 이상: multiclass
* deterministic vs. probabilistic 
    * **단정적**으로 이야기 해줌 vs **확률적**으로 이야기 해줌
    * 내부 구조의 차이가 안 날 수도 있음. 껍데기의 차이일수도! 
    * output을 중요시 하는 모델 
    * *ex> 파란색이다 vs 파란색일 확률이 80%다* 
* linear vs. nonlinear
    * bound를 선으로만 긋느냐 vs. 자유로운 곡선으로 그을 수 있냐 
* global vs. local 
    * 모델의 패턴이 너무 눈에 안보일 때 divide and conquer이 더 나을 수도 
    * -> local: 차라리 분리를 해서 모델을 각각 찾는 것(local 모델을 찾는다)
* generative vs. discriminative
    * generative: 생성 모델 : 어떤 데이터가 **데이터 공간**에 존재한다면, 어떤 임의의 데이터가 주어졌을 때 a 클래스의 데이터를 생성할 확률, b 클래스의 데이터를 생성할 확률 - 확률 모델 을 생성 
    * discriminative: 구분만 짓는 용도 
        * 위쪽은 blue, 아래쪽은 red. 라고 구분 

### Binary vs Multiclass
* **Binary** classifiers 
    * 둘 중에 하나를 선택하는 것 (e.g. +1 and -1)
    * positive class : 우리가 detect하고자 하는 관심사를 지칭할 때 (ex. spam을 찾을 때 spam을 `positivie class`라 부름, 암을 찾을 때 양성을 positive/ 흔한 경우에는 `negative`라 부름)
* **Multiclass** classifiers
    * 클래스가 두 개 이상인 것 
    * logisitic regression은 binary classes를 위해서만 디자인되었음 

### deterministic vs. probabilistic
* deterministic classifier: 단정적으로 이야기해줌 (not 생성적 모델)
* probabilistic classifier: 확률적으로 이야기해줌
    * 모델이 가지는 확신 정도를 알 수 있음, 정보량을 더 많이 얻을 수 있음
    * ex> a 클래스일 확률이 20%, b클래스일 확률이 80% 

### linear vs. nonlinear
* 모든 데이터는 벡터로 표현할 수 있음 -> classification 문제를 어떤 공간에 점이 찍혔을 때 공간을 분할하는 문제로 볼 수 있음 (빨간색? 파란색?)
    * 2차원일 때: line
    * 3차원일 때: plane
    * 4차원일 때: hyperplane 
* lienar는 선이 단순하기 때문에 복잡한 모델을 표현하기는 어려움. overfitting은 잘 안 일어남!, 대신 underfitting이 일어나기 쉬움 -> not flexible! 
* -> *일반적으로 non linear을 사용하게됨*

### global vs. local 
* global classifier: 전체 데이터를 커버하는 단 하나의 모델을 만드는 것 
* local classifier: 각 파트에 대해 여러개의 모델로 나누어서 모델을 생성 (구역별로 모델을 만들기!)

### generative vs. discriminative
* generative: 데이터 공간에 이런 데이터가 오면 각각의 클래스에 해당할 확률이 얼마나 되는지를 찾는 모델을 찾는 것 (빨간색일 확률이 몇프로, 파란색일 확률이 몇프로)
    * 마치 데이터를 생성해주는 것처럼 보임
    * ex. 이미지가 사람/고양이/개 인지 구분 -> 생성적 모델을 만들었다. (고양이/개/사람일 확률이 몇프로인지를 계산해줌) -> pixel값을 어떻게 줘야 고양이일 확률이 높을까 -> 가짜 데이터를 만드는데 많이 쓰임 (y가 될때 적절한 x값을 찾아주는 것)
    * a class를 발생시킬 확률을 찾아내는 식을 찾아주는 것 : p(y|x) x라는 데이터가 주어졌을때 y일 확률 
* discriminative classifiers
    * 구역을 나눠줌. (이쪽은 빨강, 저쪽은 파랑)

## Nearest Neighbor Classifiers 
* k-nn: 유일하게 모델을 만들지 않는 classifier
* lazy learner: 데이터가 들어왔을 때 그 데이터를 표현하는 모델을 바로 만들지 않음
    * classify하기 전까지 모델링하지 않음 
    * 모델을 만드는데 시간이 들지 않음 
* eager learner: 데이터가 들어오자마자 모델을 만들려고 함

### k-Nearest Neighbor Classifiers
* ppt 참고 

## Naive Bayes Classifier
* 대표적인 생성 모델의 아주 간단한 버전 
* classification 문제를 최초로 **확률** 문제로 표현한 논문 
    * classification 문제를 조건부 확률의 문제로 볼 수 있다! 

### Bayes Theorem
* x가 데이터값, y가 class 
* P(y|x)를 구하고자 함 -> 근데 바로 못 구함! -> 역으로 계산할 수 있음!
    * P(x|y)(데이터의 관점에서 뜻: *클래스가 y일때 x가 몇번 나왔나?*),P(y)(각각의 클래스가 발생할 확률-training data 보면 됨),P(x)(전체 데이터에서 x값이 몇 번 발생하는가) 를 통해서 쉽게 구할 수 있음

### Naive Bayes Classifier의 특징 
1. probabilistic classfication model: 확률 모델이지만 그 숫자를 믿으면 안됨. (실제 값이 아니기 때문)
2. 생성 classification model: 생성형 모델이다 
3. 독립이라는 가정을 통해 쉽게 계산 가능 
    * 독립이라는 가정을 하여 따로따로 구했을 때 장점
        * 차원이 높아지면 차원의 저주가 발생할 수 있는데, 따로따로 계산하면 차원이 높아져도 큰 문제가 발생하지 않음-> 고차원에서 문제 발생x
        * 쉽게 계산 가능
4. (복습하면서 추가할게요...필기가 바쁨)

## Bayesian (Belief) Networks
* naive Bayes는 무리한 가정(독립이라는 가정)을 했었음 -> assumption이 너무 과도한 경향이 있었음 => naive Bayes보다 유연한 모델! 

## Logistic Regression 