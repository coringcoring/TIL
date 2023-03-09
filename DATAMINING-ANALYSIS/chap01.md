# CHAP 1 

## What is Data Mining 
* `기계`의 영역 
* Data, Information, Knowledge까지 (Insight와 Wisdom은 사람이 해야할 영역이다.)
* ex> beer와 diaper 문제 
* 'Data Mining is the practice of searching through `large amounts` of `computerized data` to find useful patterns or trends(information or knowledge).'
* hidden pattern? : pattern의 모양은 개발자가 지정해줘야함을 주의. 
* 데이터 분석과 AI의 차이: 
    데이터에서 knowledge를 경영진의 선택에 이용 -> 데이터 분석 

## 4 main tasks of Data Mining 
1. `Classification`: 어떤 카테고리에 속하느냐를 예측하는 문제 (ex. 발송된 email이 스팸이냐 non스팸이냐) (classification을 조금만 변형시키면 regression이 될 수 있음. - 어떤 특정한 수치를 예측해야하면 regression)
    * 알고리즘: decision tree, naive Bayes, logistic regression, artificial neural network. ensemble methods.. 
2. `Association analysis` (연관규칙 탐색): 아이템 간의 연관성을 분석하는 문제 
    * 알고리즘: Apriori, FP-growth, sequential patterns.. 
3. `Clustering` : 비슷한 것들끼리 모으는 문제 
    * 알고리즘: k-means, hierarchical clustering, DBSCAN..
4. `Anomaly detection`: 특이한 데이터를 찾아내는 문제 (ex. outlier)
    * 알고리즘: statistical approaches, proximity-based approaches, 
clustering-based Approaches, reconstruction-based Approaches..

---

## Data Mining
* 자동적으로(`automatically`/ 컴퓨터가 스스로 하므로 자동적) `유용한` 정보(`useful` information)를 큰 데이터에서 (`large data sets`) 에서 extracting 하는 과정 
* 전통적인 데이터 분석 기법 (통계학) + 큰 데이터를 처리하는 정교한 알고리즘 (컴퓨터 사이언스)
* 모든 정보가 데이터 마이닝으로 얻은 정보는 아님. (단순 정보 검색은 데이터마이닝이라 할 수 없음. -> simple queries or simple interactions with a database system)

## 적용: 비즈니스와 산업에서..
* examples
    * point-of-sales data analysis(실제 계산이 이루지는 계산대 같은 곳에서의 쌓인 데이터 분석)
    * automated buying and selling (ex.주식)
    * customer profiling 
    * targeted marketing(쿠폰을 받아서 사용할 확률이 높은 고객 타겟팅)
    * store layout
    * fraud detection 
* 인터넷에 어마어마한 양의 데이터가 쌓이고 있음. 
    * 상품 추천, 스팸 필터링, 친구 추천 등등.. 
* 모바일 센서와 장치들이 또한 많은 양의 데이터를 만들어냄 
    * 스마트 시티, 스마트 홈 

## 적용: 과학과 공학 
* 인공위성에 의해 만들어진 지구의 정보 (지표면, 바다, 대기 정보..)
* 유전체 정보 (microarray data)
* 기록된 전자적 기록 데이터 (MRI 이미지들, ECGS..)
    * 개별화된 환자 케어 가능 

## KDD(Knowledge Discovery in Databases)의 과정 
1. `Input Data`: 텍스트 파일, 엑셀 파일, 데이터베이스(테이블) 파일.. 
2. `Data Preprocessing`: 시간이 오래 걸리는 파트. 
    * feature selection: ex. 데이터의 필드가 너무 많을 때, 필요없는 필드들을 `제거`. (필요한 필드들만 남기는 것.)
    * Dimensionality Reduction: 변환을 통해서 10개짜리 feature를 5개로 `압축`하는 것. 
    * Normalization: 데이터들을 비슷하게 만들어주는 것.
    * Data Subsetting: 데이터들이 너무 많을때 일부 row들만 선택하는 것. 
    * fusing data from multiple sources: 데이터들이 여러개 있을때 합치는 것. 
    * `cleaning data`: 가장 중요한 전처리.  
3. `Data Mining`
4. `Postprocessing` : 사후처리. 데이터 마이닝의 return을 decision maker에게 보여주기 전에 하는 것. 사람이 이해할 수 있게 가공해주어야함. 
    * 시각화 (visualization): 히스토그램, 그림 .. 
    * 가설 검증 (Hypothesis testing)
    * filtering patterns: 의미있는 패턴만 검출하여 보고
    * pattern interpretation: 패턴이 어떤 의미인지를 어떤 것인지 설명해주는 것. 

## 데이터 마이닝을 힘들게 하는 요소들
* 이번 수업에서는 깊게 다루지 않고, 이러한 이슈들이 있다는 것을 이해하고 넘어가기만 하면 됨. 
1. Scalability(확장성) : 어마어마하게 큰 데이터들을 다루어야함. 
    * 데이터가 너무 많음.. -> 검색을 좁히는 기술들
    * 디스크에 덜 접근
    * 데이터를 다 못보니까 데이터를 대표하는 몇몇개를 샘플링
    * 데이터가 너무 많으므로 많은 컴퓨터들이 일을 나눠서 처리 
2. High Dimensionality: 필드가 너무 많으면 분석이 어려워짐. -> 차원을 줄이려는 노력이 필요 
    * 차원의 저주 
    * 분석하는데 시간이 너무 오래 걸리게 됨. 
3. Heterogeneous data : 데이터 형태가 다양함. 
    * ex> text data, database table data, sequence data, graph data 
4. Data distribution 
    * 데이터를 어떻게 나누어서 각각의 컴퓨터에게 어떤 일을 시키는가. -> 분산기법 중요 