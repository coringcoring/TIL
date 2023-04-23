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
    * test data: 안 본 데이터를 통해 모델 실전 성능을 측정 -> `**good geralization performance**` 
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
2. **splitting criterion**을 사용하여 가지치기: 어떤 값으로 갈라치기를 해야하나 
    * entropy를 가장 낮추는 방향으로 갈라치기를 하는 것이 좋음 
    * splitting criterion은 **attribute test condition**을 결정지음
        * attritbute test condition의 결과에 따라 **child node**들이 만들어지고, children에게 instance들이 **분배**됨 
3. **stopping criterion**이 만족되면 가지치기를 멈춘다: 어디까지 갈라치기해야하나 
    * training data에만 딱 맞게 class 하나만 남도록 끝까지 갈라치기하는 것은 위험한 전략 -> `overfitting` 문제 발생 가능 
    * 가장 많은 training instance가 발생한 것에 해당하는 class 라벨을 leaf node에 할당 

### design issues of decision tree induction
1. `splitting criterion`: 각각의 노드에서 하나의 `attribute`과 그것의 `값`들을 training instance들을 나누기 위해 선택해야함. 
    * 어떤 `attribute`를 조건으로 택할지를 결정 
    * training instance들이 `child node`에 어떻게 분배될지를 결정 
2. `stopping criterion` : 어느 시점에서 멈출지 알 수 없음. `overfitting` 발생 전에 멈춰야. 
    * node에 있는 training instance들이 똑같은 **class label**들을 가지고 있을 때 **가지치기**를 멈출 수 있지만, **overfitting** 발생 전에 가지치기를 멈춰야함. 

### attribute test conditions 
* **`binary split`**: 2 그룹으로 나누는 것 
    * (why? *시간이 적게 걸림*, *overfitting을 방지해줌*(여러개로 나누게 되면 트리의 깊이가 더 깊어지고 모델이 복잡해지므로 **overfitting**이 발생할 확률이 높아지게됨))
* **`multiway split`** : 여러개로 나누는 것 
    * `numeric attribute` : range로 split함  
    * `categorical attribute` 

### selecting an attribute test condition
* 가능한 한 `pure`한 노드를 찾자
* `혼잡도`를 계산 -> `엔트로피` 사용 (혼잡도 계산하는 다른 방법들도 존재)
    * `**pure node**` : node에 있는 training instance들이 모두 똑같은 class들을 가짐
    * `**impure node**` : node에 있는 training instance들이 여러개의 class들을 가짐 
    * impure한 노드들은 `트리의 깊이`를 깊어지게 하고, 큰 트리들은 `model overfitting`을 유발할 가능성이 높음  

### impurity measures for a single node 
1. **`엔트로피`**: 값들의 `information`과 `uncertainty`의 평균
    * `발생확률`이 클수록 `정보량` 감소 
2. **`지니 계수 (Gini Index)`**: 값들 사이의 `inequality` 정도 
    * 모든 값들이 똑같으면 `minimized`(`perfect equality`)
    * 모든 값들이 가능한한 모두 다르면 `maximized` 
3. **`classification error`**: 가장 많은 class들에 속하지 않은 instance들의 비율 
* 모두 `binary` classification problem에서 distribution이 `uniform`할 때 impurity가 `maximized`됨    
    * instance들이 모두 하나의 class에 대응할때 impurity는 모두 `minimized`됨 

### collective impurity of cihld nodes
* ppt 참고 

### finding the best attribute test condition
* `goodness` of an `attribute test condition`을 결정해야함 
    * 가지치기를 하고 얼마나 `impurity`가 **감소**하는지를 측정해야함
    * 많이 impurity가 감소할수록 좋은 test condition이 됨 
* **`information gain`**을 계산 : Information Gain = `I(parent)`-`I(children)`
    * I(parent): 가지치기 하기 전에 부모 노드의 impurity
    * I(children): 가지치기 하고 나서 자식 노드들의 impurity
    * entropy로 impurity 계산 예 (ppt) 참고
    * gini index 도 마찬가지로 ppt참고 

### Binary Splitting of Numeric Attributes
* 기본적인 방법 순서 (ex. annual income)
    1. annual income에 따라 training instance들을 분류
    2. 후보를 나눌 수 있도록 `midpoint`를 선택 
    3. 각각의 split된 후보들의 `information gain`을 계산
    4. 가장 높은 `information gain`을 만들어내는 position을 선택 

### decision tree classifier의 특징들 
1. **`applicability`**
    * 어떤 `distribution`이든 잘 작동함 (잘 가르는가? 만 보기 때문에 `skewed`되어도 잘 가르기만 하면 되므로 문제가 안됨.)
    * `전처리`가 딱히 필요 없다. (**categorical**, **numeric** data 둘다 적용 가능)
    * **multiclass problems** 또한 처리 가능 (class가 많아져도 사용가능) (cf.**logistic regression**은 multiclass problem 쓸 수 없음.)
    * 유도된 트리는 *해석하기가 쉬움* 
2. **`expressiveness`**
    * x랑 y가 **discrete**하면 함수로 만들어줄 수 있다 -> 표현이 가능하다! 
    * 가지만 늘리면 powerful한 모델이 되지만 `overfitting` 문제 발생 -> 해결: 여러개의 모델을 만들어서 취합 후 정확성을 높임 (`앙상블` 기법)
3. **`computational efficiency`**
    * 많은 decision tree 유도 알고리즘들은 `heuristic`한 접근을 사용 
        * ex> greedy, top-down(위에서 아래로 내려오는), recursive partitioning strategy
    * 데이터가 **많고** **어려운** 상황에서 적절함 (`빠르기` 때문!)
    * 데이터가 크더라도 **빠르게** decision tree를 적당히 좋게 만들어낸다 
    * test instance들을 분류하는 속도가 **매우 빠르다** 
4. **`handling missing values`** 
    * **non-missing** value들로 missing value를 가지고 있는 instance를 child node에 배정 가능 or missing value를 가지고 있는 instance들을 **배제**함 
5. **`handling irrelevant attributes`**
    * y를 결정하는데 상관없는 attribute 가 많다면.. decision tree는 잘돌아갈까? -> *잘 안돌아간다*! (우연히 y가 잘 갈라질 수도 있겠지만.. **잘못된 판단**을 할 수 있음)
    * 관련 없는 attribute를 꼭 처리해줘야! (전처리 강조)
6. **`handling redundant attributes`**
    * 거의 **동일한** 정보를 가지고 있는 attribute (`redundant`)
    * 이것에 대해서는 decision tree가 잘돌아감. (하나가 선택되면, 또다른 하나는 나중에 또 select될 확률이 **낮아지므로**)
7. **`using rectilinear splits`**
    * **test condition**은 각각 하나의 **attribute**만을 포함하기 때문에, `decision boundary`들이 `rectilinear`하다 (`coordinate axes`에 `평행`하다)
        * `decision boundary`?: 다른 class들의 지역을 구분짓는 경계선 
    * decision tree는 `사선`을 흉내내기 위해 가지를 계속 쳐야함.. 
    * `복잡한` 데이터 패턴을 알아내기가 어려움 
8. **`choice of impurity measure`**
    * 어떤 impurity measure을 선택하더라도 성능에 큰 차이는 **없음**
        * 이유: measure들이 꽤 서로 **비슷함** 
    * 대신, `stopping condition` [일정 수준 이하의 entropy가 확보되면 decision tree 가지치기를 멈춤 (limit을 거는것) (or depth가 어느정도 내려왔을때 stopping condition)] 은 `final tree`에 큰 영향을 준다. 
        * 이유: tree의 `overfitting`과 큰 관련이 있기 때문 

---

## General Issues of Classification Models
1. Model Overfitting 
2. Model Selection: 복잡도가 어느정도 되어야 하는 모델인가? 
3. Model evaluation: 2번 과정을 통해서 선택한 모델이 얼마나 잘 돌아가는가 
4. Presence of hyper-parameters : 2번과 4번 관련되어 있음(복잡도 관련),  parameter는 모델이 학습을 통해 알아내는 값, hyper-parameter은 사람이 지정해야하는 parameter(기계가 학습을 할 수 없으므로) 
    * hyper-parameter의 종류는 다양함, hyper-parameter들 중 모델의 복잡도를 결정하는 요소들이 있음 (hyper-parameter가 2번보다 더 큰 개념이라 생각하세요)
    * ex. neural network을 쓰면 layer을 몇개 두어야할지, neuron의 갯수는 몇개로 할지 등은 사람이 지정해줘야함 (hyper-parameter 중 모델의 복잡도를 결정하는 요소)

### 1. Model Overfitting
* 너무 과도하게 `training data`에 fit된 문제 -> poor한 generalization performance를 보여줄 때 
* model이 overfitting 되었는지는 어떻게 알아보나? 
    * the performance on `test data` < the performance on `training data`
        * 이것이 너무 크게 차이가 나면 `overfitting`
* **model underfitting** : 너무나 **단순**한 모델을 써서 **training data**조차 제대로 묘사를 하지 못하는 경우 발생 
    * 해결방법: model의 **복잡도**를 높여야함. (너무 복잡도를 올리면 overfitting 발생할수도..) or **data**를 많이 넣어주기 
* 발생하는 이유
    1. **`limited training size`**
        * 트레이닝 데이터를 너무 작게 주면 test data에 대한 성능이 안좋게 나올 수 밖에 없음 
        * training set은 전반적인 data에 대한 **제한된** 측면만을 제공해줌 -> 완전히 **전반적인 데이터**의 **pattern**을 모두 보여줄 수는 없음 => **training data size**를 늘리면 overfitting이 **감소**하게됨 
        * 요즘은 이런 경우 없음 
    2. **`high model complexity`**
        * **데이터**가 별로 없는데 너무 과도하게 복잡도를 높였을 경우 -> **데이터**에 비해 모델이 너무 **복잡**할때 (너무 **specific한 패턴**들을 잡아버리게 됨)
* 모델 복잡도를 측정하는 법? 
    * **`parameter의 개수`** = 모델의 복잡도 (많을수록 복잡도 증가, 유연성이 증가)
        * **decision tree**: **node**의 개수 (=**attribute test condition**의 개수)
        * **linear aggression**: **coefficient**의 개수 
        * **neural network**: **가중치**의 개수 (**뉴런**, **layer**의 개수)
    * **parameter**가 너무 많은데 **training data**가 너무 **작다**면.. -> 임의의 데이터가 정답으로 간주될 수도 있음 (다른거 묘사를 못하게됨, **overfitting**) (just by random chance)
### 2. Model Selection
* 보지 않은 데이터에 대해서 잘 **일반화**시켜주고, 적당한 레벨의 **복잡도**를 가지는 모델을 선택하는 것 -> **training error rate**만 가지고 모델을 선정해서는 안된다. *다른 **3가지 접근**을 고려해야한다.* 
#### (1) valdiation set 사용 
* Data = `D.tr`+ `D.val`+ `D.test` 
    * `D.tr`: **모델**을 만들때 사용함
    * `D.val` : 최소의 **hyper-parameter** 선택할 때 씀 (**모델**을 선택할 때), generalization performance를 측정 
        * error rate on D.val 은 **`validation error rate`** 
    * `D.test`: 선택 완료 후, **성능** 측정할때 사용 
* 한계
    * `D.tr`이 너무 작으면, `poor model`
    * `D.val`이 너무 작으면, 한쪽이 너무 치우치기 때문에 믿을 수가 없음 (+ `validation error rate`가 **적은** 수의 **instance**들로 계산이 되기 때문에)
        * **2/3**으로 나누면 적당하다 
#### (2) pre-pruning (only for decision tree): 가지를 치기 전에 멈추는 것 
* **가지**를 치다가 **혼잡도**가 충분히 떨어지면 멈춤 (충분한 혼잡도가 갖춰지면 멈춤)
* 더 **제한적인** **stopping condition**이 사용되어야함 
* 장점: **training data**의 너무 **복잡한** **subtree** 생성을 피함  
* 단점: **최선의 성능**을 가지리란 보장이 **없음** (다 안열어봤기 때문)
#### (3) post-pruning (only for decision tree)
* 하나씩 접어 올라가면서 접었을때 **validiation** 에 대한 성능이 더 좋은지 살펴보는 것
* 접었는데 좋으면 계속 접어들어감 (성능이 **안좋아질때까지** 계속 접어감)
* trimming 전략 1: **`subtree replacement`** 
    * instance들의 대다수 **class label**이 결정되어지면 **subtree**를 하나의 **leaf node**로 대체 
* trimming 전략 2: subtree raising (시험 문제 안낸다)
* 장점: **pre-pruning**보다 더 좋은 **결과**를 줄 수 있음 
    * 이유?: **tree-growing**에서 **premature한 termination**으로부터 벗어날 수 있음
* 단점: **full tree**를 만들기 위해 **추가적인 연산**이 소요됨 
### 3. Model Evaluation 
* **unseen 데이터**에 대한 성능을 측정하기 위해 별도의 테스트 수행 
* `validation error rate`는 모델의 성능을 평가하는데는 **`biased indicator`**이다 
    * **validation error rate**는 모델을 고를때 사용했었음 -> 이는 validation set에서 error rate를 가장 최소화하는 model을 **의도적**으로 골랐을 거임 -> 이는 **true generalization error rate**라고 볼 수 없음. 믿을 수 없는 evaluation
* a correct approach for model evaluation 
    * 한번만 테스트하는 것으로는 불안함 -> 여러번 **테스트**하여 `평균`을 측정하여 성능을 더 정확히 측정 (`cross-validation`)
* **`holdout method`**(한번만 하는거)/ **`cross-validation`**(여러번 테스트 하는거)
    * **`Holdout Method`** 
        1. D를 D.train, D.test로 나눔 
        2. D.train으로부터 모델을 선택하고 train
        3. D.test에 대한 일반적인 성능을 측정 
    * D.train과 D.test의 비율 
        * 통상적으로 D.train:D.test=**3:1**이면 적당 
    * **`random subsampling`** (or **`repeated holdout method`**)
        * hold out을 여러번 해서 성능을 측정 
        * error rate의 **분포**를 얻어낼 수 있음 -> **generalization performance**에 대한 **variance**를 이해 가능 
    * **`cross-validation`**
        * 모든 데이터에 대해서 training, test를 한번씩 수행 가능 
        * 모든 데이터는 반드시 **한번의 test**에 참여하게 되어있음 
        * 모든 데이터는 **training**에 **k-1**번 참여하게 됨
        * 각각의 test data는 몇번 test에 쓰이고 .. 가 시험에 나옴 
        * **`k-fold cross validation`** (ppt 참고)
        * the right choice of **k** 
            * k값을 너무 **작게** 잡으면.. **training data**가 줄어듦 -> 모델의 최고점의 성능을 찍지 못하게됨(*성능이 안좋아짐*)
                * **E**가 높게 측정됨 
            * k값이 너무 **크면** .. : **시간**이 너무 오래 걸림 
                * E가 **편향**이 감소됨 
            * *k값은 보통 최소 5이상, 10 정도를 많이 사용함* 
            * **`Leave-one-out approach`**  
                * **k= N** (data size) 인 익스트림한 케이스   
                * 장점: **training**하는데 데이터를 가능한한 많이 활용 가능 
                * 단점: **시간**이 너무 오래걸림 
            * 보통 k는 5~10 사이로 사용됨 : 전체 데이터의 **80~90%**의 데이터를 사용할때 reasonable함 
        * **E**라는 것은 **model selection approach**의 **일반적인 error rate**의 **예측값** 
        * 각각의 실행에서 다른 모델이 학습 된다 -> k개의 다른 모델들의 error rate를 **평균**한것이 **E** (E는 k개의 모델 중 그 어느 것이라도 **error rate**를 **대표하지 못함**)
        * E는 **model selection approach**에서 일반화된 기대되어지는 **error rate**를 반영함 (D.train(i)의 똑같은 크기의 training set에 적용될때)
        * **holdout method**에서 구해진 **error rate**(**D.train**에 대해 **specific**한 모델)와는 **다름** 
### 4. Presence of Hyper-Parameters 
* `Hyper-parameters`: 모델을 만들어주기 전에 결정해주어야하는 `learning algorithms`의 parameter들 
    * **model selection**할때 결정되어야함
* cross-validation 확장 : validation에 대한 cross-validation 
* hyper-parameter: 몇개로 할 것인가?가 대표적인 하이퍼 파라미터. 
1. simple approach 
    * p값의 후보를 줘야함 
    * D.train을 D.tr과 D.val로 나눔 
    * D.val 하나만 보고 parameter을 선택하는 것은 불안함
2. cross-validation : validation에 대한 cross-validation
3. nested cross-validation 
* 위험성
    * **training set**과 **test set**은 겹치면 안됨! -> test data의 error rate가 보지 않은 데이터들의 수행을 대표한다고 보기 어려워짐 
    * **validation error**를 **generalization error**로 사용하면 안됨! -> validation error rate는 unseen instance들에 대한 performance를 보여줄 수 없음! 