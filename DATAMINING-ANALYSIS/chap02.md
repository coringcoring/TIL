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
* 각각의 데이터 간에 어떠한 명시적인 관계가 없음.
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
    * 원값을 사용하는 것이 아니라 바운더리를 끊어, 바운더리 주변의 값에서 평균을 구하여 부드러운 곡선으로 변형-> 후에 패턴을 찾는데 도와줌, 변동을 줄일 수 있음. 
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
    2. 동명이인처럼 값이 중복해서 들어갈때. (JKL 동명이인일수도)

## 데이터 전처리 (Data Preprocessing)
* 아주 넓은 영역의 데이터를 다뤄야
* 7가지 주요 전처리 작업들:
    1. Aggregation
    2. Sampling : 데이터가 너무 많아서 대표적인 것들 몇개만 추려내 모델을 만드는 것. 
    3. Dimensionality reduction: 차원 압축(감소). 차원이 많아지면 분석 결과가 의미 없어지므로. 
    4. Feature selection: 필요 없는 feature들을 날리고 필요한 feature들만 골라서 모델을 만드는 것. 
    5. Feature creation: domain knowledge 필요. 데이터를 가공하여 새로운 feature을 만들어주는 것. (ex. 어떤 물질의 부피와 무게가 주어졌을때 밀도 feature를 만들어줌)
    6. Discretization and binarization: 이산화(숫자(numerical)인 attribute를 categorical attribute로 바꿀 때), categorical attribute를 numericla attribute로 바꾸는 것 
    7. Variable transformation 
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