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

## voiting approaches
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