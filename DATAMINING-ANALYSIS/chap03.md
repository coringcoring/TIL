# CHAP 03

## Classification 
* categories로 분류 
* 데이터
    * x = (x1,...,xn)
    * y= categorical label 
* 모델 : x와 y 사이의 관계를 추상적으로 표현 
* ex> loan borrower classification 
    * supervised learning (지도 학습)
* general framework
    1. classification
    2. classifier = model
    3. training set 
    4. learning algorithm(학습 알고리즘): 모델을 만들어내는(규칙을 찾아내는) 알고리즘
    5. induction: 귀납, 일반화된 rule을 찾는 것 
    6. deduction: 연역, 각각의 건에 대해 모델을 적용하여 예측하는 것

### performance of a classifier 
* data set을 training set과 test set으로 분할해야함. 
    * training data: 모델을 만드는데 사용 
    * test data: 안 본 데이터를 통해 모델 실전 성능을 측정 -> **good generalization performance** 

### evaluation metrics 
* confusion matrix 
    ex> accuracy, error rate, recall 

---
## Decision Tree Classifier
* 주어진 데이터에서 트리를 찾아내는 것이 목표
* 어떤 attribute를 선택하느냐가 문제 
* 설명이 편한 model. 

### structure of decision tree 
* **attribute test condition** 

### classifying an unlabeled instance
* knn 알고리즘은 실시간 처리 불가능 (너무 느림)

### algorithms to build a decision tree
* attribute가 많아질수록 exponential 해짐 (여러가지의 가짓수가 생겨버림!)
    * `optimal`을 찾아내는 것은 어려움 
* 효과적인 알고리즘은 적당히(reasonably) 정확함
    * greedy strategy 대개 사용 
    * 그 시점에서 최선(`locally optimal`)
* ex> hunt's algorithm , ID3(+information gain), C4.5(numeric도 다루게됨), CART(classification + regression(숫자를 예측) tree) 

### Hunt' algorithm
1. root node에서 시작 
2. **splitting criterion**: 어떤 값으로 갈라치기를 해야하나 
    * entropy를 가장 낮추는 방향으로 갈라치기를 하는 것이 좋음 
3. **stopping criterion**: 어디까지 갈라치기해야하나 
    * training data에만 딱 맞게 class 하나만 남도록 끝까지 갈라치기하는 것은 위험한 전략 -> overfitting 문제 발생 가능 

### design issues of decision tree induction
1. splitting criterion
2. stopping criterion : 어느 시점에서 멈출지 알 수 없음. overfitting 발생 전에 멈춰야. 

### attribute test conditions 
* binary split: 2 그룹으로 나누는 것 (why? 시간이 적게 걸림)
* multiway split : 여러개로 나누는 것 
    * numeric attribute 
    * categorical attribute 

### selecting an attribute test condition
* 혼잡도를 계산 -> 엔트로피 사용 
    * pure node : 가능한 한 pure하는 노드를 찾자
    * impure node 

### impurity measures for a single node 
1. 엔트로피
2. 지니 계수 (Gini Index)
3. classification error

### finding the best attribute test condition
* information gain을 계산 
    * entropy
    * gini index 

### decision tree의 장점
1. applicability
    * 어떤 distribution이든 잘 작동함 (잘 가르는가? 만 보기 때문에 skewed되어도 잘 가르기만 하면 되므로 문제가 안됨.)
    * 전처리가 딱히 필요 없다. (categorical, numeric data 둘다 적용 가능)
    * multiclass problems 또한 처리 가능 (class가 많아져도 사용가능) (cf. logistic regression은 multiclass problem 쓸 수 없음.)
    * *해석하기가 쉬움* 
2. expressiveness
    * 가지만 늘리면 powerful한 모델이 되지만 overfitting 문제 발생 -> 해결: 여러개의 모델을 만들어서 취합 후 정확성을 높임 (앙상블 기법)
3. computional efficiency
    * 데이터가 많고 어려운 상황에서 적절함 (빠르기 때문!)
    * ex> greedy, top-down(위에서 아래로 내려오는), recursive partitioning strategy
4. handling missing values 
5. handling irrelevant attributes
    * y를 결정하는데 상관없는 attribute 가 많다면.. decision tree는 잘돌아갈까? -> 잘 안돌아간다! (우연히 y가 잘 갈라질 수도 있겠지만.. 잘못된 판단을 할 수 있음)
    * 관련 없는 attribute를 꼭 처리해줘야! (전처리 강조)
6. handling redundant attributes
    * 거의 동일한 정보를 가지고 있는 attribute (redundant)
    * 이것에 대해서는 decision tree가 잘돌아감. (하나가 선택되면, 또다른 하나는 나중에 또 select될 확률이 낮아지므로)
7. using rectilinear splits
    * decision tree는 사선을 흉내내기 위해 가지를 계속 쳐야함.. 
    * 복잡한 데이터 패턴을 알아내기가 어려움 
8. choice of impurity measure
    * 어떤 impurity measure을 선택하더라도 성능에 큰 차이는 없음 
    * 일정 수준 이하의 entropy가 확보되면 decision tree 가지치기를 멈춤 (limit을 거는것) (or depth가 어느정도 내려왔을때 stopping condition)

## General Issues of Classification Models
1. Model Overfitting 
2. Model Selection: 복잡도가 어느정도 되어야 하는 모델인가? 
3. Model evaluation: 2번 과정을 통해서 선택한 모델이 얼마나 잘 돌아가는가 
4. Presence of hyper-parameters : 2번과 4번 관련되어 있음(복잡도 관련),  parameter는 모델이 학습을 통해 알아내는 값, hyper-parameter은 사람이 지정해야하는 parameter(기계가 학습을 할 수 없으므로) 
    * ex. neural network을 쓰면 layer을 몇개 두어야할지, neuron의 갯수는 몇개로 할지 등은 사람이 지정해줘야함 (hyper-parameter)

### 1. Model Overfitting
* 너무 과도하게 training data에 fit된 문제 
* model이 overfitting 되었는지는 어떻게 알아보나? 
    * the performance on test data < the performance on training data
        * 이것이 너무 크게 차이가 나면 overfitting
* model underfitting : 너무나 단순한 모델을 써서 training data조차 제대로 묘사를 하지 못하는 경우 발생 
    * 해결방법: model의 복잡도를 높여야함. (너무 복잡도를 올리면 overfitting 발생할수도..) or data를 많이 넣어주기 
* 발생하는 이유: 가진 데이터에 비해서 모델의 복잡도가 너무 강해서 overly complex 하기 때문 
    * 너무 specific한 패턴들을 잡아버리게 됨 
