# CHAP 03

## Classification 
* categories로 분류 
* 데이터
    * x = (x1,...,xn)
    * y= categorical label 
* 모델 : x와 y 사이의 관계를 `추상적`으로 표현 
* ex> loan borrower classification 
    * attribute들은 type 제한이 없음 / but y는 `categorical type`이어야! 
    * 모든 attribute들이 classification task와 관련이 있지 `않음` (ex. ID)
    * `supervised learning` (`지도 학습`)
* general framework
    1. `classification` : 라벨이 붙여져 있지 않는 instance들에게 label을 붙이는 task
    2. `classifier` = `model` 
    3. `training set`    
        * 모델을 만들기 위해 주어진 instance들의 set 
        * attribute 값들+ 각각 instance들의 `class label`들을 가지고 있음 
    4. `learning algorithm`(학습 알고리즘): 모델을 만들어내는(규칙을 찾아내는) 알고리즘
    5. `induction`: `귀납`, 일반화된 rule을 찾는 것 -> `학습 알고리즘`을 사용하여 training data로부터 `모델`을 만들어내는 것 
    6. `deduction`: `연역`, 각각의 건에 대해 모델을 적용하여 예측하는 것 -> 모델을 적용하여 보지 않은 instance들에 대해 class label을 예측하는 것 

### performance of a classifier 
* 예측한 label과 실제 label을 비교함으로써 classifier(=model)을 평가 가능 
* data set을 `training set`과 `test set`으로 분할해야함. 
    * training data: 모델을 만드는데 사용 
    * test data: 안 본 데이터를 통해 모델 실전 성능을 측정 -> `**good generalization performance**` 
        * 모델은 보지 않은 데이터에 대해서도 정확하게 class label들을 예측해내야함 
        * training set로 평가하는 것은 **generalization performance**의 좋은 방법이 아님 

### evaluation metrics 
* `confusion matrix` : classifier(=model)의 performance 평가의 결과를 묘사한 테이블 
    ex> accuracy, error rate, recall 
    * `정확도(Accuracy)` = 맞은 prediction의 수 / 전체 예측의 수 
    * `에러율 (error rate)`= 잘못된 prediction의 수 / 전체 예측의 수 
    * ppt 그림 참고 

---
## Decision Tree Classifier
* 주어진 데이터에서 **트리**를 찾아내는 것이 목표
    * **class label**을 찾아낼때까지 
    * `decision tree`: 여러개의 질문들, 가능한 답들이 **계층적**으로(hierachical) 조직된 구조
* 어떤 `attribute`를 선택하느냐가 문제 -> 여러 개의 instance의 속성들에 대한 질문들을 통해 classification 문제를 해결 
* **설명**이 편한 model. 

### structure of decision tree 
* `root node` : `0개` 이상의 link들 연결되어 있음
* `internal node`: `1개`의 들어오는 link, `2개` 이상의 나가는 link가 연결됨
* `leaf(terminal) node`: 정확히 `1개`의 들어오는 link, 나가는 link가 `없음` 
* **attribute test condition** : **root**와 **internal node**에 포함되어 있음, *해당 노드의 자식 중 정확히 하나에 대응된다는 것을 의미* 

### classifying an unlabeled instance
* knn 알고리즘은 실시간 처리 불가능 (너무 느림)
* 기본적인 순서
1. `root node`에서 시작
2. `attribute test condition`을 적용하여, test의 결과에 근거한 적절한 `가지`를 따라간다
3. `leaf node`에 도달하면, instance에게 node에 해당되는 `class` 값을 할당해줌 

### algorithms to build a decision tree
* **attribute**가 많아질수록 `exponential` 해짐 (여러가지의 가짓수가 생겨버림!)
    * `optimal`한 data set을 찾아내는 것은 어려움 
    * 많은 decision tree들은 particular한 data set에 의해 만들어질 수 있음 
* 효과적인 알고리즘은 `적당히(reasonably) 정확함`
    * `greedy strategy` 대개 사용 -> decision tree를 top-down 방식으로 키움 
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
* 혼잡도를 계산 -> 엔트로피 사용 (혼잡도 계산하는 다른 방법들도 존재)
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
    * hyper-parameter의 종류는 다양함, hyper-parameter들 중 모델의 복잡도를 결정하는 요소들이 있음 (hyper-parameter가 2번보다 더 큰 개념이라 생각하세요)
    * ex. neural network을 쓰면 layer을 몇개 두어야할지, neuron의 갯수는 몇개로 할지 등은 사람이 지정해줘야함 (hyper-parameter 중 모델의 복잡도를 결정하는 요소)

### 1. Model Overfitting
* 너무 과도하게 training data에 fit된 문제 
* model이 overfitting 되었는지는 어떻게 알아보나? 
    * the performance on test data < the performance on training data
        * 이것이 너무 크게 차이가 나면 overfitting
* model underfitting : 너무나 단순한 모델을 써서 training data조차 제대로 묘사를 하지 못하는 경우 발생 
    * 해결방법: model의 복잡도를 높여야함. (너무 복잡도를 올리면 overfitting 발생할수도..) or data를 많이 넣어주기 
* 발생하는 이유
    1. limited training size
        * 트레이닝 데이터를 너무 작게 주면 test data에 대한 성능이 안좋게 나올 수 밖에 없음 
        * 요즘은 이런 경우 없음 
    2. high model complexity
        * 데이터가 별로 없는데 너무 과도하게 복잡도를 높였을 경우 -> 데이터에 비해 모델이 너무 복잡할때 (너무 specific한 패턴들을 잡아버리게 됨)
* 모델 복잡도를 측정하는 법? 
    * parameter의 개수 = 모델의 복잡도 (많을수록 복잡도 증가, 유연성이 증가)
        * decision tree: node의 개수 
        * linear aggression: coefficient의 개수 
        * neural network: 가중치의 개수 (뉴런, layer의 개수)
    * parameter가 너무 많은데 training data가 너무 작다면.. -> 임의의 데이터가 정답으로 간주될 수도 있음 (다른거 묘사를 못하게됨, overfitting) (just by random chance)
### 2. Model Selection
* Data = D.tr+ D.val+ D.test 
    * D.tr: 모델을 만들때 사용함
    * D.val : 최소의 hyper-parameter 선택할 때 씀 (모델을 선택할 때)
        * error rate on D.val 은 validation error rate 
    * D.test: 선택 완료 후, 성능 측정할때 사용 
* 한계
    * D.tr이 너무 작으면, porr model
    * D.val이 너무 작으면, 한쪽이 너무 치우치기 때문에 믿을 수가 없음 
    * 2/3으로 나누면 적당하다 
* pre-pruning (only for decision tree): 가지를 치기 전에 멈추는 것 
    * 가지를 치다가 혼잡도가 충분히 떨어지면 멈춤 (충분한 혼잡도가 갖춰지면 멈춤)
    * 장점: training data의 너무 복잡한 subtree 생성을 피함  
    * 단점: 최선의 성능을 가지리란 보장이 없음 ( 다 안열어봤기 때문)
* post-pruning (only for decision tree)
    * 하나씩 접어 올라가면서 접었을때 validiation 에 대한 성능이 더 좋은지 살펴보는 것
    * 접었는데 좋으면 계속 접어들어감 (성능이 안좋아질때까지 계속 접어감)
    * trimming 전략 1: subtree replacement 
    * trimming 전략 2: subtree raising (시험 문제 안낸다)
### 3. Model Evaluation 
* unseen 데이터에 대한 성능을 측정하기 위해 별도의 테스트 수행 
* a correct approach for model evaluation 
    * 한번만 테스트하는 것으로는 불안함 -> 여러번 테스트하여 평균을 측정하여 성능을 더 정확히 측정 (cross-validation)
* holdout method(한번만 하는거)/ cross-validation(여러번 테스트 하는거)
    * Holdout Method 
        1. D를 D.train, D.test로 나눔 
        2. D.train으로부터 모델을 선택하고 train
        3. D.test에 대한 일반적인 성능을 측정 
    * D.train과 D.test의 비율 
        * 통상적으로 D.train:D.test=3:1이면 적당 
    * random subsampling (or repeated method)
        * hold out을 여러번 해서 성능을 측정 
* cross-validation
    * 모든 데이터에 대해서 test를 한번씩 수행 가능 
    * 모든 데이터는 반드시 한번의 test에 참여하게 되어있음 
    * 각각의 test data는 몇번 test에 쓰이고 .. 가 시험에 나옴 
    * the right choice of k 
        * k값을 너무 작게 잡으면.. training data가 줄어듦 -> 모델의 최고점의 성능을 찍지 못하게됨(성능이 안좋아짐)
        * k값이 너무 크면 .. : 시간이 너무 오래 걸림 
        * k값은 보통 최소 5이상, 10 정도를 많이 사용함 
        * Leave-one-out approach    
            * 장점: training하는데 데이터를 가능한한 많이 활용 가능 
            * 단점: 시간이 너무 오래걸림 
            * 보통 k는 5~10 사이로 사용됨 
    * E라는 것은 model selection approach의 일반적인 error rate의 예측값 
### 4. Presence of Hyper-Parameters 
* cross-validation 확장 : validation에 대한 cross-validation 
* hyper-parameter: 몇개로 할 것인가?가 대표적인 하이퍼 파라미터. 
1. simple approach 
    * p값의 후보를 줘야함 
    * D.train을 D.tr과 D.val로 나눔 
    * D.val 하나만 보고 parameter을 선택하는 것은 불안함
2. cross-validation : validation에 대한 cross-validation
3. nested cross-validation 
* 위험성
    * training set과 test set은 겹치면 안됨! 