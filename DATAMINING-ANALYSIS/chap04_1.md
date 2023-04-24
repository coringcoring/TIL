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
    * 모델이 가지는 확신 정도를 알 수 있음, **정보량을 더 많이 얻을 수 있음**
    * ex> a 클래스일 확률이 20%, b클래스일 확률이 80% 

### linear vs. nonlinear
* 모든 데이터는 벡터로 표현할 수 있음 -> classification 문제를 어떤 공간에 점이 찍혔을 때 공간을 분할하는 문제로 볼 수 있음 (빨간색? 파란색?)
    * 2차원일 때: line
    * 3차원일 때: plane
    * 4차원일 때: hyperplane 
* lienar는 선이 단순하기 때문에 복잡한 모델을 표현하기는 어려움. overfitting은 잘 안 일어남!, 대신 underfitting이 일어나기 쉬움 -> not flexible! 
* -> *일반적으로 non linear을 사용하게됨*

### global vs. local 
* global classifier: `전체 데이터`를 커버하는 단 하나의 모델을 만드는 것 
* local classifier: `각 파트`에 대해 여러개의 모델로 나누어서 모델을 생성 (구역별로 모델을 만들기!)

### generative vs. discriminative
* `generative`: 데이터 공간에 이런 데이터가 오면 각각의 클래스에 해당할 확률이 얼마나 되는지를 찾는 모델을 찾는 것 (빨간색일 확률이 몇프로, 파란색일 확률이 몇프로)
    * 마치 데이터를 생성해주는 것처럼 보임
    * ex. 이미지가 사람/고양이/개 인지 구분 -> 생성적 모델을 만들었다. (고양이/개/사람일 확률이 몇프로인지를 계산해줌) -> pixel값을 어떻게 줘야 고양이일 확률이 높을까 -> 가짜 데이터를 만드는데 많이 쓰임 (y가 될때 적절한 x값을 찾아주는 것)
    * a class를 발생시킬 확률을 찾아내는 식을 찾아주는 것 : p(y|x) x라는 데이터가 주어졌을때 y일 확률 
* `discriminative classifiers`
    * 구역을 나눠줌. (이쪽은 빨강, 저쪽은 파랑)

---

## Nearest Neighbor Classifiers 
* **`k-nn`**: 유일하게 모델을 만들지 않는 classifier
* `lazy learner`: 데이터가 들어왔을 때 그 데이터를 표현하는 모델을 바로 만들지 않음
    * classify하기 전까지 모델링하지 않음 
    * 모델을 만드는데 시간이 들지 않음 
* `eager learner`: 데이터가 들어오자마자 모델을 만들려고 함

### k-Nearest Neighbor Classifiers
* ppt 참고 
* k-NN classifier의 특징
    1. `instance-based learning` : 모델을 따로 생성하지 X. 적절한 proximity measure가 필요됨(instance들 사이의 유사도를 구하기 위해서)
    2. `비용`이 많이 든다 
        * 이유: 각각의 데이터의 `유사도`를 다 측정해주어야하기 때문에 오래걸림
    3. `local information에 근거`하여 예측값을 찾아냄 
        * 모델을 안만들고 주변 데이터들만을 이용함
        * 전체 데이터에 맞는 global model을 만드는 decision tree와 다르다
        * 따라서 k-NN classifier들은 `noise에 취약함` 
    4. 임의의 모양의 decision boundary들을 만듦
        * decision tree에서 rectilinear decision의 boundary보다 더 유연함 -> `overfitting에 취약함` 
        * k값을 늘리는 것은 유연성을 잃는 것 
    5. **`missing value를 training set, test set에서 다룰 수 없음`** 
        * 이유: 근접성(거리) 계산은 `모든 attribute`가 존재해야만 일반적으로 가능하기 때문 
    6. **`irrelevant attribute는 proximity measure를 왜곡할 수 있음`** 
        * 특히 irrelevant attribute의 수가 많을 때 왜곡 많이 함 
        * `irrelevant, rebundant attribute`들은 k-NN classifier의 수행 능력에 영향을 미침 
    7. `적절한 proximity measure`와 `전처리`가 이루어지지 않으면 잘못된 prediction을 만들어낼 수 있음 
        * 정규화(normalization) 없이 proximity를 측정할 때 가중치에 큰 영향을 받을 수 있음 -> 이러한 가중치의 영향을 줄이는 작업이 필요함. (k-NN은 이걸 자동으로 할 수 없음)
---

## Naive Bayes Classifier
* 대표적인 생성 모델의 아주 간단한 버전 
* classification 문제를 최초로 **확률** 문제로 표현한 논문 
    * classification 문제를 조건부 확률의 문제로 볼 수 있다! 

### Bayes Theorem
* x가 데이터값, y가 class 
* P(y|x)를 구하고자 함 -> 근데 바로 못 구함! -> 역으로 계산할 수 있음!
    * P(x|y)(데이터의 관점에서 뜻: *클래스가 y일때 x가 몇번 나왔나?*),P(y)(각각의 클래스가 발생할 확률-training data 보면 됨),P(x)(전체 데이터에서 x값이 몇 번 발생하는가) 를 통해서 쉽게 구할 수 있음

### Bayes Theorem 이용하여 classfication 문제 해결하기
* computing P(y|x1,x2,...xd)를 계산할때의 문제
    1. n(x|y)와 nx를 세야함 
    2. attribute 개수가 많아지면 같은 attribute value들을 가지는 training instance들은 적어지게 됨 -> P(x1,x2,...xd|y)와 P(x1,x2,..xd)의 결과가 좋지 못함 (0에 가까워지는 값..) (특히나 training size가 작을때 많이 발생함)
        -> 해결방법: x1,x2,..xd가 모두 서로 독립이라고 가정 

### P(xi|y)에서 continuous한 attribute를 계산하는 법
1. categorical attribute로 바꾼다 
2. 특정한 확률분포의 형태로 가정한다 

### Naive Bayes Classifier의 특징 
1. probabilistic classfication model: 확률 모델이지만 그 숫자를 믿으면 안됨. (실제 값이 아니기 때문)
2. 생성(generative) classification model: 생성형 모델이다 
3. 독립이라는 가정을 통해 쉽게 계산 가능 
    * 독립이라는 가정을 하여 따로따로 구했을 때 장점
        * 차원이 높아지면 차원의 저주가 발생할 수 있는데, 따로따로 계산하면 차원이 높아져도 큰 문제가 발생하지 않음-> 고차원에서 문제 발생x
        * 쉽게 계산 가능
4. 이상한 noise 값에 대해서도 강건함 
    * noise에 큰 타격을 받지 않음. 이상한 한건이 있다해도 큰 영향을 주지 않는다 
5. missing value를 다룰 수 있다. 
    * training set-> P(xi|y) 계산하면서 그냥 무시함 
    * test set ->  non-missing attribute value들만 사용한다 (=그냥 제거하고 계산하면 됨)
6. irrelevant attribute에 강건하다 
    * irrelevant attribute는 모든 클래스에 각각 균등하게 분포하고 이는 성능에 큰 영향을 주지 않음 
7. data가 추가되면 몽땅 다시 만들어야 하는 다른 모델(decision tree등) 과 달리 data가 들어오는 족족 모델 업데이트가 가능함 

---

## Bayesian (Belief) Networks
* naive Bayes는 무리한 가정(독립이라는 가정)을 했었음 -> assumption이 너무 과도한 경향이 있었음 => naive Bayes보다 유연한 모델! 
* DAG(Directed Acyclic Graph)로 표현됨 
* Bayesian Netwokrs의 특징들 
    1. *attirbute들과 class들 사이의 확률적인 관계*를 강력하게 잘 나타내준다 
        * varaible 사이의 의존관계의 복잡한 형태도 잘 나타냄
    2. 조건부적으로 독립에 대한 가정(naive Bayes에서는 가정했던)을 하지 않음 => *corrleated 또는 rebundant한 attribute*들을 다룰 수 있음 
    3. naive Bayes classifier처럼 training data에서 *noise와 irrelevant한 attribute*를 잘 다룰 수 잇음 
        * training+testing 할때 *missing value*들 또한 잘 다룰 수 있음 
    4. Bayesian network의 *topology(structure)*를 알아내는 것은 어려움 
        * 전문적인 지식 (도메인 지식)이 필요됨 
        * 일단 topology가 만들어지면, probabilistic table를 바로 얻어낼 수 있음
    5. **naive Bayes classifier에 비해 `overfitting에 취약함`** 
        * 각각의 probability table을 채우기 위해 많은 training data가 필요함. -> `data가 작으면` naive Bayes에 비해 overfitting이 일어날 가능성이 높음. 각 `probability table`의 값이 달라질 수 있기 때문
---
## Logistic Regression 
* logistic regression의 특징들 
    1. 독립이다와 같은 가정을 만들지 않고 P(y=1|x)를 계산하여 0.5보다 크다/작다만을 판단하는 `discriminative 모델`
    2. **학습된** `파라미터`들은 w0,...wn -> `attribute`와 `class` 사이의 관계를 이해하는데 도움을 준다 
        * wn이 크다 -> 어떤 `파라미터`가 크다 -> 어떤 `attribute`가 더 큰 영향을 준다 
    3. `irrelevant attribute`들을 다룰 수 있다 
        * 미리 판단할 필요 없다. logistic regression이 알아서 irrelevant한 attribute weight에 0에 가까운 값을 자동적으로 줄 것임
    4.` rebundant attribute`들을 다룰 수 있다 
        * LR이 알아서 weight로 attribute들을 조정한다. (**똑같은** weight값을 줌)
    5. `missing value`들을 다룰 수 **없다**! 
        * P(y|x)는 오직 1/(1+z)에 의해 계산되어짐-> missing value 때문에 0.5보다 큰/작이 달라줄 수 있기 때문 
