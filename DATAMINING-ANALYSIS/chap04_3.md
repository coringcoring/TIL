# CHAP 04_3

## Ensemble Method (앙상블 기법)
* 다수의 classifier들의 예측 결과들을 모음으로써 classification의 정확도(accuracy)를 늘림 
* base classifiers
* 대표적으로 다수결의 원칙을 사용하여 aggregation 수행 

## 앙상블 classifier을 위해 필요한 2가지 조건 
1. base classifier들은 서로 독립이어야 한다 (error들이 uncorrelated여야 한다)
2. base classifier들은 정확도가 0.5 이상이어야 한다. 

## ensemble 만드는 방법 
1. manipulating training set 
    * 원래 data에서 랜덤하게 데이터를 sampling하여 뽑기(resampling)하여 여러개의 training set들을 만듦 
    * 그 각각의 training set으로 부터 classifier을 만듦
    * bagging: random sampling을 하여 각각의 sampling에 대해 만들어냄 (단점: 시간 많이 듦)
    * boosting : 앞의 모델들에서 잘 못 맞춘 것들을 위주로 샘플링을 하여 모델을 만들어냄. bagging과 달리 모델들이 독립적이지 못하지만, 뒤의 모델들을 더 효과적으로 만들어낼 수 있음 
2. manipulating the input features: feature들을 random하게 선택 
    * 각각의 training set으로부터 input feature들의 subset을 랜덤하게 뽑음 (혹은 도메인 지식 사용)
    * 그 각각의 training set으로부터 classifier 만듦
    * random forest : record, feature도 랜덤하게 만들어줌 -> 모든 decision tree가 독립적으로 만들어지게 됨 
3. manipulating the learning algorithm: hyper parameter를 랜덤하게 선택 
    * 똑같은 training data에 알고리즘을 여러번 적용 
    * 서로 다른 classifier들의 construction에 영향을 끼침 
    * ANN(다른 network topology들이나 initial weight를 사용)
    * decision tree(다른 splitting strategies 사용[gini index, entropy, classification error 중에서 하나를 쓴다든지..])

## voting approaches
1. simple majorty voiting: 다수결 
2. weighted majority voting: 가중치를 주는 다수결 
    * 가중치: accuracy(가장 많이 사용), importance or relevance 

## bias and variance
1. bias: 정답과 내가 예측한 답이 차이가 생기는 것
    * 편향이 많이 되어 있다 -> 모델이 너무 심플할 때 자주 발생 (ex. logistic regression)
    * underfitting 발생 가능 -> 살릴 수 없음
2. variance
    * 너무 민감하다 -> overfitting 발생 가능 -> 살릴 수 있음 (ex. ANN)
        * ensemble 기법에서 bias는 낮고 variance는 높은 model을 여러개 반영하는 것이 좋음 -> variance를 낮출 수 있음 (다수결을 사용하기 떄문에)
* 복잡한 모델 -> 낮은 bias, 높은 variance -> overfitting
* 단순한 모델 -> 높은 bias, 낮은 variance -> underfitting
* ensemble 기법은 낮은 bias, 높은 variance인 base classifier들을 사용할때 가장 좋은 결과를 보여준다. (오히려 overfitting을 시켜줘야하는 것)
* 다수의 base classifier들의 결과를 결합하면서, ensemble 기법은 variance를 줄여줄 수 있다 

## Bagging
* Bootstrap AGGregatING
* 가장 단순한 방법
* 기본 과정 
    1. 반복적으로 sample을 data set에서 만들어냄 (replacement 사용!)
        * replacement: sampling을 하고 다시 넣어야함 -> 똑같은게 또 뽑힐 수 있음! -> 데이터가 모두 서로 다르게 뽑히는 것 
        * 모든 data를 uniform(균등하게) 뽑는다 (모든 데이터는 뽑힐 확률이 동일)
        * sample의 개수 = 원래 데이터의 수 (데이터가 n개면 sample도 n개)
    2. 각각의 bootstrap sample로부터 base classifier을 학습
    3. 가장 많은 수의 vote를 받은 class를 test instance에 할당해줌 
* 3가지 조건
    1. 각각의 instance는 똑같은 확률로 뽑힌다
    2. 각각의 bootstrap sample은 original data와 같은 크기다
    3. sampling은 replacement로 된다 
* sample은 원본 dataset의 63% 정도가 sampling 되어 나온다 (37%는 안뽑힌다)
    * 이유: 각각의 sample은 1-(1-1/n)^n (한번은 뽑힐 확률) 의 선택될 확률을 가짐 
    * n이 충분히 크면, 1-1/e는 0.632에 가까워짐 -> 통상 63%밖에 안뽑힘!
* 단점: 꽤 많은 decision tree들을 만들어야함-> 시간이 오래 걸림 (해결방법: boosting 사용)
* boundary가 수직 or 수평 -> ensemble기법을 사용하면 좀 더 유연하게 보이게 됨 (bias가 낮은 애들도 모아놓으면 유연하게 움직이는 것이 보임)
* bagging은 base classifier들의 variance를 줄여줌으로써 generalization error 를 개선시킴 
    * base classifier가 높은 variance라면: bagging은 training data에서 랜덤한 변동들을 만들어낼 수도 있지만, 다른 애들이 이를 잡아주므로 error를 줄일 수 있다
    * base classifier가 높은 bias라면: bagging은 base classifier들의 성능을 개선시킬 수 없다. -> 모델의 훈련 데이터가 오히려 줄어드는 효과 

## boosting
* 앞의 결과들을 보고 weight를 adaptively하게 변경시킨다. 
    * 장점: classify하기 어려운 example에게 더 집중할 수 있음
    * 단점: 각각의 모델이 독립적이지 않음 -> 병렬적으로 설정할 수 없음 -> 시간이 오래 걸린다 
* weight는 sampling distribution을 만들어내고, 가장 많이 biased된 example들에게 더 높은 weight를 주게 됨. 
* 기본 과정 
    1. training example들에게 모두 똑같은 weight (1/n)을 할당
        * weight의 합은 1이 되도록 
    2. sampling distribution에 따라 sample을 그림 
    3. sample로부터 각각 classifier을 만들고 원본 데이터에 있는 모든 example들을 classify하는데 사용한다 
    4. training example들의 weight를 update한다 
        * 잘못 classified -> weight 증가
        * 맞게 classified -> weight 감소 
    * 가급적 weight가 높은 애들은 틀리지 않으려고 노력함 
    * classify하기 어려운 example들에 집중함 
### AdaBoost
* boosting algorithms
    1. 각각의 round에서 training example들의 weight가 어떻게 update되는지
    2. 각각의 classifier들의 예측 결과들이 어떻게 결합되는지에 따라 
        알고리즘이 달라짐 
* adaboost는 Adpative Boosting이다
* 나머지 내용 pdf 참고
* classifier가 잘못 만들어져서 error rate가 0.5보다 크게 되면 -> 모든 training data에 대한 rate를 **초기화시켜야함** (다시 하자!) -> weight를 다 1/n로 초기화 
-> 그래야 importance는 0보다 크다고 가정이 가능  
### boositng의 특징 
* ensemble의 training error는 지수적으로 감소함 -> 알고리즘의 fast convergence를 이끔 
* 잘못 분류된 (분류되기 어려운) example에 집중함으로써 bias가 높더라도 잘 돌아감(학습이 잘됨)-> variance를 낮추는 효과 
* 단점: overfitting에 취약함
    * 잘못 학습된 data(ex. outlier)가 있따면, 이상한(특이한) 데이터에 치중하게 되는 결과가 나타나게됨 -> 잘못된 데이터에 최적화될 수도 있음(overfitting) (<-> random forest) 

## Random Forests
* bagging + spliting point를 random하게 뽑는 과정 
* 현실적으로 아주 좋은 성능을 가지는 모델 
* decorrelated한 decision tree들을 ensemble하여 generalization performance를 개선시킴 
* random하게 서로 다른(독립적인) decision tree들을 만듦 => 다수결 => prediction 
* bagging과 같은점: bagging과 동일한 sampling을 함. training data에서 다른 sample을 만듦
* bagging과 차이점: randomly하게 선택된 attribute 사이에서 best splitting criterion을 고름 
* random forest construct ensembles of decision trees by.. 
    1. training instance들을 뽑음 (training instance의 63%를 뽑음) 
        bagging과 비슷한 boostrap sample들을 뽑음 
    2. input attribute들을 모든 internal node에서 다른 attribute의 subset을 사용하여, splitting할때마다 sampling을 새로 하여 선택함 (서로 다른 splitting attribute가 나오게 됨)
* random forest classifier을 training 하는 과정 
    1. training set D: n개의 instance들과 d개의 attribute들 
    2. bootstrap sample Di를 만든다 
        * randomly하게 replacement를 사용하여 n개의 instance들을 sampling
    3. Di를 decision tree Ti를 학습하기 위해 사용함 
        * *splitting할 때마다 p개의 attribute들을 random하게 뽑아서 splitting함*
        * Ti의 모든 internal node에서 randomly하게 p개의 attribute set을 고름
        * information gain이 가장 높은 attribute subset을 고름 
        * **`이 절차를 모든 leaf node가 pure해질 떄까지 반복함`**
            * 트리의 깊이가 깊어짐 -> 각각의 model에 대한 variance를 극대화시키기 위해(overfitting을 시켜도됨 -> 의도적으로 발생시키는 것)
            * 모든 leaf가 pure할 때까지 끝까지 splitting! 
                * ensemble을 variance가 높은 것들을 모아 결과를 도출하므로.. 
* random forests의 특징 
    * 예측: ensemble of deicision tree들이 만들어지면, 그들의 평균적인 prediction(`majority vote`)가 final prediction에 사용됨 
    * RF에 있는 decision tree들은 `unpruned tree`들이다 
        * 즉, 모든 leaf node가 pure해질떄까지 가능한 한 tree가 계속 사이즈가 커질 수 있음 
        * 그러므로 RF의 base classifier들은 낮은 bias, 높은 variance를 가지고 있음 
    * RF에 있는 base classifier들은 `not correlated` (서로 모양이 많이 다르다)
        * `독립적인(서로 다른) sampled data set`을 사용하므로 (bagging 접근방법과 비슷함)
        * splitting할때 randomly하게 선택된 `서로 다른 attribute subset`들을 사용하므로   
        * 이 것은 tree 들 사이의 모양을 서로 많이 다르게 함 
    * tree들 사이의 lack of correlation
        * 만약, 많은 attribute들을 가진 training set에서 적은 subset of attributes 만이 target class의 strong predictors라면. (강한 영향을 주는 attribute가 계속 선택됨)
        * 다른 bootstrap sample들을 뽑아낸다하더라도, internal node에서 splitting을 할 때 strong predictor인 똑같은 attribute들만 뽑아내게 될 것임 -> tree들 사이에 상당한 correlation이 발생하게 됨 
        * 그러나, 우리가 모든 internal node에서 choice of attribute들을 제한한다면, tree들 간의 다양성을 높일 수 있음 
            * 이유: strong, weak predictor 둘다의 선택을 보장할 수 있게 됨 
            * 결론적으로 decorrelated한 decision tree들을 만들어낼 수 있다
    * random forests의 장점
        * random forests는 low bias에 영향을 주지 않고 variance를 낮춰줄 수 있다 
            * strong(variance가 높은)하고 decorrelated(서로 다른) decision tree들의 ensemble의 prediction들을 모음으로써 
        * random forest들은 overfitting에 강함 (boosting과 달리!)
            * 이유: 매우 복잡한 tree들에 대해 variance를 낮춰줄 수 있기 때문 
        * random forest들은 계산이 빠르고, 고차원 환경에서도 강함 
            * 만드는 비용이 크지 않음 
            * 이유: attribute가 아무리 많더라도 attribute는 항상 p개를 먼저 고르기 때문
    * hyper-parameter 
        1. 트리를 몇개를 만들어야 하는가?
        2. p : 모든 노드에서 선택되어야하는 attribute들의 개수 
            * p가 너무 작으면?: 트리의 모양이 엄청 달라지게 됨, 강력한 트리가 나올 수 없음 (이상한 트리가 나올 수 있음)
            * p가 너무 크면? : 트리의 모양이 서로 비슷해짐, 각각의 tree는 강력한 놈이 나올 수 있음 
    * random forests vs. AdaBoost
        * Random forests: generalization performance에서 상당히 좋은 개선을 보여준다. (Adaboost보다 더 나은 성능)
        * **`random forests는 또한 Adabbost에 비해서 overfitting에 강함`** 
        * random forest는 빠르다 

## Multiclass problem
* 몇몇 classification 기술들은 binary classification 문제들에 designed되어 있음 
* 그러나 실세계에서는 input data는 실제로 2개 이상의 category들로 나뉘어짐 
* binary classifier들을 확장함으로써 multiclass 문제들을 다룰 수 있을 것이다 
1. One-Against-Rest (or All): 하나 또는 그외(또는 나머지)
    * multiclass problem을 k개의 binary problem들로 분해 
    * ppt 참고하기 
2. One-Against-One 
    * k(k-1)/2 (combination임)개의 binary classifier들을 만듦. 
    * 자세한 설명 ppt 참고 