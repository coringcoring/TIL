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
    * test data: 안 본 데이터를 통해 모델 실전 성능을 측정 

### evaluation metrics 
* confusion matrix 
    ex> accuracy, error rate, recall 

---
## Decision Tree Classifier
* 주어진 데이터에서 트리를 찾아내는 것이 목표
* 어떤 attribute를 선택하느냐가 문제 
* 설명이 편한 model. 

### structure of decision tree 
* attribute test condition 

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