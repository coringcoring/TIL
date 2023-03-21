# CHAP 02 

## 용어
* Data Object (=record, [point, vector -> 각각의 column의 값이 모두 숫자일 경우 쓸 수 있음.], case, sapmle, instance, observation..)
* Attribute (=variable, field, feature, demension)
* Data set 

## 데이터 마이닝에서 데이터 관련된 쟁점들 
1. 데이터의 타입 구분하기 
    * 텍스트(categorical(정해진 값들 중 하나), numeric(randomly하게 오는 숫자값들..))? 숫자? 
    * 데이터셋은 다른 특징들을 가지고 있음. (record data, graph data: record끼리 관계가 있을 때, ordered data: 시간적 관계가 있을 때)
2. 데이터의 품질 
    * 데이터는 완벽하지 않다. (노이즈, 비정상 데이터, 데이터 missing, 중복 데이터 .../ 한쪽으로 치우친 데이터 또는 대표하기 어려운 데이터)
    * 데이터의 품질을 향상시키고 이해하는 것이 결과의 품질을 높인다. 
3. 전처리 (Preprocessing)
    * 분석에 알맞게 raw data를 처리해야.
    * 특정 기술(알고리즘)에 딱 알맞도록 데이터를 수정. 
4. 유사도 측정
    * 데이터의 유사도를 어떤 방식으로 측정할 것인가. 

## Attribute 타입
1. categorical : 
    * 정해진 것들 중에서만 올 수 있는 것. (limited)
    * 임의의 숫자로는 연관 규칙을 찾아내기 어려우므로 categorical로 바꾸는 것이 필요함. (ex. value가 50이상인지/아닌지->categorical)
    * nominal: 그냥 unique한 symbol일 경우 
    * ordinal: 크기 비교 가능한 column
    * 절대 continuous가 될 수 없음. discrete
2. numeric 
    * continuous가 될 수 있음. 
3. discrete (ex. 나이)
4. continuous (ex. 온도-> 35.5, 37.2 .. 다 가능함.)
* record data: 각각의 데이터가 독립적. 아무런 관계가 없음
* graph-based data: 데이터 간에 어떤 관계가 있을 때 
* ordered data: 데이터 간에 순서관계가 있을 때

## 데이터 셋의 특징 
* 데이터를 받으면 데이터의 차원 개수가 어떤지 확인하고, 분포가 잘 되어 있는지 확인 
* 차원:
    * 차원의 개수를 줄여야함. 
    * 차원의 저주 
* Distribution
    * 특정 값들이 한쪽으로 편향되어 있는지? 
    * 특정 column에 있는 값들이 어떻게 분포되어있는지 파악할 필요 있음. (ex. 데이터가 정규분포를 따를때에만 적용되는 경우가 있음)
### (1) Record data
* 각각의 데이터 간에 어떠한 명시적인 관계가 없음. (독립적)
* 전처리 과정이 필요됨. 
    * record data
    * transaction data
    * data matrix(모든 값이 숫자 -> vector, point라 부를 수 있음) 
    * documet-term matrix: 문서 당 단어들이 몇 번 나오는지.. 
### (2) Graph-Based Data
* case 1> 데이터 간에 관계가 존재 
    * 노드, 간선이 굉장히 많음 
    * 링크 
    * ex> WWW(World Wide Web)(노드: 웹페이지, 간선: 하이퍼링크), 사회 네트워크망 
* case 2> 그래프로 된 데이터들 
    * 각각의 데이터가 하나의 그래프
    * ex> 화학 화합물(간의 유사도, 많이 발생하는 서브 그래프 찾기 등..)
### (3) Ordered Data 
* 데이터간에 순서가 있을때. 
* case1> 순서
    * 타임스탬프 (분석할때 시간 순서를 고려해야할 때)
* case2> 타임 시리즈
    * 시간에 따른 측정값들을 리스팅할 때 
    * temporal autocorrelation 고려해야함. 
* case3> 연속적인 데이터 
    * 타임 스탬프가 없음. 
    * 서브 패턴, 유사한 염기서열 등을 찾음.
    * 바이오인포매틱스 
    * 텍스트 데이터, 유전 데이터 등 
* case4> 공간적인 데이터 
    * spatial: 공간 
    * 나의 위, 아래, 옆에 어떤 값인지.. 
    * 공간별로, 시간별로 데이터가 쌓이고 있음 -> spatial autocorrelation 를 고려해야. 

## Data Quality
* 데이터는 완벽하지 않음. 
    * 측정 기기의 한계, 데이터 수집 과정에서의 결함, 사용자가 일으키는 오류 등 
* 데이터 품질 문제를 해결하기 위해 
    1. `data cleaning` : 데이터 품질 문제를 발견하고 고침 
    2. 잘못된 데이터가 왔을때 알고리즘에서 이를 감안하여 처리 

## 측정, 데이터 수집 과정에서의 오류 
* 측정 오류 : 특정 row에서 column 값들이 빠져있다거나 등..
* 데이터 수집 오류

## Noises and Artifacts 
* Noise 
    * 원 데이터를 사용하는 것이 아니라 바운더리를 끊어, 바운더리 주변의 값에서 평균을 구하여 부드러운 곡선으로 변형-> 후에 패턴을 찾는데 도와줌, 변동을 줄일 수 있음. (노이즈 정리,제거)
* Artifacts 
    * ex> 사진에 찍힌 하얀 줄 

## Outliers
* 애매모호한 정의: 흔하지 않은 값을 가진 데이터. (그러나 이게 정상 데이터일수도, 오류 데이터일 수도 있음.) 

## Missing Values
* 버려야함. (column 혹은 row 자체를 날려버릴 수도 있음.)
* interpolation: 버리기 아까운 경우.. 주변 값들로 빠진 값을 대강 예측하여 넣을 수 있음. 
* 분석할때 빠진 값을 무시해버림 

## Inconsistent Values
* 일관성이 없는 값들 
* 잘 찾아서 이러한 문제들을 고쳐주어야함. 

## Duplicate Data
* 중복 데이터 
* 두가지 이슈들 
    1. 동일한 객체에 대해서 다른 값으로 들어갈 때. (JKL이 동일인물일수도)
    2. 동명이인처럼, 실제로는 서로 다른 값일때. (JKL 동명이인일수도)

## 데이터 전처리 (Data Preprocessing)
* 아주 넓은 영역의 데이터를 다뤄야
* 7가지 주요 전처리 작업들:
    1. Aggregation
    2. Sampling : 데이터가 너무 많아서 대표적인 것들 몇개만 추려내 모델을 만드는 것. 
    3. Dimensionality reduction: 차원 압축(감소). 차원이 많아지면 분석 결과가 의미 없어지므로. 
    4. Feature selection: 필요 없는 feature들을 날리고 필요한 feature들만 골라서 모델을 만드는 것. 
    5. Feature creation: domain knowledge 필요. 데이터를 가공하여 새로운 feature을 만들어주는 것. (ex. 어떤 물질의 부피와 무게가 주어졌을때 밀도 feature를 만들어줌)
    6. Discretization and binarization: 이산화(숫자(numerical)인 attribute를 categorical attribute로 바꿀 때), categorical attribute를 numericla attribute로 바꾸는 것 
    7. Variable transformation : (vairable = feature) 값을 통째로 변환시킬 때 
1. Aggregation
    * 압축 
    * 2개 이상의 데이터들을 하나의 데이터로 결합시킴 
    * 장점 
        1. 메모리를 덜 차지하고, 처리 시간이 줄일 수 잇음 
        2. 원본 데이터를 압축함으로써 데이터를 이해하는데 도움을 줌 -> high-level view of the data 
        3. 변동성이 큰 데이터일때 변동성을 줄여줄 수 있음 -> more stable
    * 단점  
        * 얼마나 어디까지 압축해야할지를 분석 목적에 따라 파악해야함. -> 압축으로 인한 loss 발생 가능하므로 
2. Sampling
    * 데이터가 너무 많으므로 일부 데이터들을 뽑아서 모델을 돌림. 
    * 대표성이 있는 데이터들을 뽑아야함. 
        * sampling한 데이터와 전체 데이터의 특성이 대략적으로 같아야함. (ex. 평균)
    * sampling approaches
        1. simple random sampling 
            * 2가지 기법: sampling without replacement, sampling with replacement 
            * 문제가 있음: 데이터가 치우쳐있을때 랜덤하게 뽑으면 편향되지 않은 데이터가 안뽑힐 가능성이 있음. -> stratified sampling로 해결
        2. stratified(층) sampling 
            * 클래스에 비례해서 sampling을 뽑는 것 
            * 각각의 그룹에 대해서 일정량, 비례하게 sampling을 뽑아줘야함. 
    * sample 의 사이즈를 정하는 것이 어려움 -> Progressive(or Adaptive) Sampling 사용 
        * 적은 샘플에서 큰 샘플 사이즈로 증가시키면서 성능을 관찰 (어느 시점 이후부터 성능이 더이상 증가하지x, 평탄해짐. (`level off`)-> 충분한 샘플링을 했다!)
3. Dimensionality Reduction
    * ex. `PCA(Principal Component Analysis)` : 수학적으로 구한 선과 실제 점들 사이의 차이를 최소화하는 선을 찾는다(not linear regression. linear regression은 세로축으로 거리를 짓지만 PCA는 선과 수직이 되도록 하여 거리를 잼)-> 점들을 선에 붙여버림 -> 일차원으로 차원 축소. (원본 데이터의 성질은 가능한한 그대로 유지, 압축을 하기 때문에 어쩔 수 없이 손실은 일어남.)
    * 좋은점
        1. 차원이 축소될수록 알고리즘이 잘 작동 (차원의 저주)
        2. 더 이해하기 쉬운 모델을 얻어낼 수 있음
        3. 데이터를 쉽게 시각화하여 보여줄 수 있음
            ex> 3차원을 2차원으로 축소하여 시각화 가능 
        4. 요구되는 시간과 메모리를 축소할 수 있음 (데이터의 크기를 줄였기 때문)
    * 차원의 저주 
        * 차원이 늘어남-> 공간이 폭발적으로 늘어나지만, 데이터는 그대로 있음 -> 데이터가 띄엄띄엄 존재하게 됨: `sparse`
        * classification에서 문제: 차원이 커지면서 빈 공간이 많아짐>공간에 데이터가 없어짐 
        * clustering에서 문제: 공간이 넓어질수록 점(값)들 간의 간격이 넓어지게 됨-> 점들의 거리가 멀어짐-> 의미있는 clustering이 안됨. 
4. Feature Selection
    1. Rebundant features: 통계학적으로 상관 관계가 높은 feature들 날리기
    2. Irrelevant feature: 거의 필요가 없는 정보를 가진 feature들 날리기
    * approaches to feature selection
        1. domain knowledge를 사용 또는 직감적으로 판단 
        2. Embedded approaches: 알고리즘 그 자체가 어떤 attribute를 쓸지 결정함 (ex. decision trees)
        3. filter approaches: 알고리즘 돌리기 전에 correlation이 적은 attribute 택함
        4. wrapper approaches: 성능을 가장 좋게만드는 단독 필드를 찾고, 다음 second field를 찾고 .. 어느정도 충분히 늘어나서 성능이 더 이상 증가하지 않는 시점까지의 필드들을 택함. 
    * feature weighting: 일단 넘어감 (안함!)
5. Feature Creation
    * 새로운 feature을 만들어주는 것 (domain knowledge가 필요됨..)
    * feature extraction 
        * ex> 물질의 부피, 무게 주어졌으나 이것만으로 task 수행 어려움-> 무게, 부피 이용해 밀도 feature 새로 만들어줌 
        * domain knowledge가 요구됨
    * mapping the data to a new space 
        * ex> euclidean 공간 (x,y)에서는 decision tree로 classify하기 어려움 -> polar coordinate systme(r,세타) 공간으로 변환시켜 적용함 
        * 변환을 해야 모델이 더 좋은 결과를 찾아줄 때 사용 
6. discretization and binarization
    * discretization(이산화): 임의의 숫자가 나온 attribute 값들(연속적인 값들)을 범주화시킴. 
        * 어떻게 범위를 나누는지가 문제. 
        * simple approaches
            * equal width discretization: 데이터가 한축에 많이 모여있을때 한끗 차이로 다른 범주가 되어버릴 수도 있음..
            * equal frequency discretization: 간격마다 같은 갯수의 값들이 각각 들어가도록..
            * clustering-based discretization: cluster에 따라 범주를 나누어주는 것 
    * binarization(이진화): binary 로 바꾸어주는 것 
        * 단순 넘버링은 위험할 수 있음. -> 가능한한 이진화를 사용 (0,1)
        * simple technique: 해당 feature가 있으면 1, 없으면 0 -> `원한? 인코딩`
7. Variable(=feature) transformation
    1. simple functions: 스케일링 효과. 간단한 수학적 function 적용시킴 
        * ex> 주로 로그 사용하여 숫자를 다운시킴
        * ex2> 데이터를 normal distribution 로 바꾸고자 할때 제곱근, 로그, 1/x 많이 사용함 
    2. normalization(정규화) or standardization

---

## 유사도/비유사도 측정 
* 근접도(Proximity): 얼마나 두 개체가 유사한가 (유사도,비유사도 둘다 포함하는 개념), 많은 proximity measures가 존재(어떤 measure를 쓸지 결정해야..)
* 유사도: 유사할수록 값이 크게 나옴 
* 비유사도: 유사할수록 값이 작게 나옴 
* EX> Proximity Measures
    1. [Dissimilarity] 거리: Manhattan distance, Euclidean distance, Supremum distance..
    2. [Simliarty] Simliarity coeffiecients: Simple matching coefficient, Jaccard coefficient 
    3. [Simliarity] Cosine simliarity
    4. [Similarity] Correlation
    5. [Similarity] Mutual information
1. Distance 
    * 데이터는 vector로 바뀌어 있다고 전제. 
    * 어떤 거리 측정법을 사용할지를 데이터/ 데이터 분석목적에 맞게 개발자가 설계/혹은 결정해야함. 
    * Euclidean distance: x,y좌표의 차이를 유의미하게 반영하고 싶을 때 사용하는 거리 측정법 (항상 통하는 거리 측정법은 아니다.)
    * Minkowski distance
        1. L1 norm: Manhattan distance (r=1)
        2. L2 norm: Euclidean distance (r=2)
        3. L3 norm: Supremum distance (r= $$infty$$)
            * attribute 중에서 가장 차이가 많이 나는 거리를 알고 싶을 때 사용 
    * 거리의 특징
        1. Positivity: 양수여야함.
        2. Symmetry: x에서 y까지의 거리 y에서 x까지의 거리는 일치해야함. 
        3. Triangle inequality: 삼각형에 관한 부등식 -> clustering할때 문제 발생하는거 방지 가능 (x와 z사이 거리가 너무 먼데 x,y,z가 같은 cluster로 묶어지는 문제가 발생하는 걸 삼각형에 관한 부등식을 통해 방지 가능 )
        => 이러한 특징들은 우리의 직관에 대한 결과를 잘 표현해줄 수 있음.  
2. Simliarity Coefficients 
    * SMC(Simple matching coefficient): 전체 개수를 모두 센 다음에 같은 비트를 세주는 것 
    * J(Jaccard coefficient): presences들만 세주는 것. 
3. Cosine similarity
    * x와 y 사이의 angle을 측정 
    * 내접값 
    * x의 길이 y의 길이-> normalization
    * 같은 칸에 같은 워드가 많이 나올수록(0이 아닌 값) 내접값 증가 
4. Correlation 
    * 관련성이 있는가 
    * 많은 correlation type이 존재 : 이번 수업에서는 Pearson's correlation 
    * Pearson's Correlation
        * standard_deviation: 0~1사이 값으로 표현하기 위해 존재
        * covariance: (x,y)가 있을때 평균에서 얼마나 떨어져있는지를 x,y 동시에 고려해주는 것. (x가 커질수록 y가 커지면 covariance값이 커짐)
            * scale을 타기 때문에 normalization이 필요됨. 
            * preprocessing할때 많이 쓰임: rebundant feature 제거할때 corr을 계산하여 1에 가까운 값-> 움직임이 똑같은 값이므로 필요가 없음 -> 제거! 
        * ex> 증가 패턴이면 corr 그대로... 
5. Mutual information
    * mutual information=1 : x값이 y값을 결정하는데 영향력이 세다 
        * distinct한 number가 몇개인지에 따라 최대값은 1보다 더 커질 수 있음 
    * mutual information=0 : x값이 y값을 결정하는데 영향력이 없다 
    * 이쪽의 정보가 다른쪽의 정보에 얼마나 영향을 미치는지를 보여줌 
