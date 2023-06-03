# CHAP 07

## Cluster Analysis(Clustering)
* 데이터를 유사한것들끼리(ppt참고) group들로(cluster들) 나누는 것 
* cluster의 2가지 목적
    1. 이해(understanding): 데이터의 natural한 구조를 이해하기 위해서
        * goal: 자동적으로 `potential`한 class들(cluster들)을 찾는 것 -> 공통의 특징들을 공유하는 object들의 개념적으로 의미있는 그룹들을 찾는 것 
        * 적용 예시 
            * Biology 
            * Information retrieval
            * Climate
            * Psychology
            * Medicine
            * Business
    2. 활용(Utility): 데이터를 압축하기 위한 용도로 clustering함 
        * goal: 가장 대표적인(represntative)한 cluster prototype을 찾는 것 -> cluster에서 가장 대표적인 data object를 찾는 것 
        * 적용 예시 
            * Summarization
                * data set 전체를 오로지 cluster prototype으로 구성된 data set으로 줄임(reduce) -> 데이터 분석에서 시간, 공간 복잡도를 줄일 수 있음 
            * Compression
                * 각각의 cluster prototype에 integer value(index)를 할당 (cluster 무슨 번호인지만을 저장)
                * 그 cluster와 연관된 prototype의 index로 각각의 object를 대표하도록 함 
            * Efficiently finding nearest neighbors
                * 원래는 가장 가까운 neighbor들을 모든 점들 사이의 거리를 계산함으로써 찾아냄 -> 그 대신에 가장 가까운 prototype들을 찾아서 그 cluster 안에 있는 point들만 고려하면 됨  
* Cluster Analysis: object들과 그들의 관계를 묘사하는 데이터에서만 발견된 information에 근거하여 data object들을 그룹하는 것 
    * goal
        * 같은 그룹 (cluster) 안에 있는 object들은 서로 비슷함(similar)
        * 다른 그룹에 있는 object들은 서로 다름(different)
    * 좋은 clustering 조건
        * 그룹 안에서 similarity를 극대화하는 것 
        * 그룹 사이에서 difference를 극대화하는 것
* clustering의 어려움 
    * cluster의 개념은 잘 정의되어 있지 않음 -> cluster의 정의가 imprecise(명확하지 않음)
    * 가장 좋은 정의는 원하는 결과들(desired results)와 data의 nature에 따름 
* classification vs clustering
    * 공통점: clustering은 classification의 한 형태로 간주될 수 있음 -> class label(cluster label)들로 object들을 labeling하는 것이 가능 
    * 차이점
        * classfication은 supervised classification(typical usage)임 
            * label이 무엇인지 알려져있음
        * clustering은 unsupervised classification임 
            * label들은 오로지 data로부터 얻어낼 수 있음 
* segmentation, partitioning vs clustering
    * segmentation과 partitioning은 때때로 clustering과 동의어로 쓰임: 전통적인 cluster analysis의 외부 접근으로서 빈번하게 사용됨 
    * partitioning: 종종 사용자가 준 어떤 기준(certain criteria)(clustering과 강한 연관은 없음)에 따라 data를 나눌때 사용됨
    * segmentation: 종종 간단한 기술을 사용해서 data를 그룹으로 나눌때 사용함 
        * ex. 딥러닝에서의 image segmentation
        * ex. 사람들을 비슷한 특징을 가진 사람들끼리 묶는 것 
## clustering의 다른 type들 
1. partitional clustering(분할)
    * 겹치는 subset(cluster)들 없이(non-overlapping subsets(clusters)) 데이터 object들을 나누는 것 
2. hierarchical clustering(계층적)  
    * tree로 조직되어질 수 있는 중첩된(nested) cluster들의 set 
3. exclusive clustering: 각각의 object가 하나의 (single) cluster로 할당됨
4. overlapping(or non-exclusive) clustering: 하나의 object가 1개 이상의 cluster에 속할 수 있음(애매한 객체가 있는 경우 해석이 더 편하게)
5. fuzzy clustering: 모든 object들이 모든 cluster에 membership weight와 함께 속함
    * 0~1 사이의 값 (1이면 absolutely belongs)
    * 첫번째 cluster에 속할 확률, 두번째 cluster에 속할 확률 .. 
6. complete clustering: 모든 점을 억지로 cluster에 넣음 (outlier가 있어도 억지로 cluster에 넣음) -> k means의 단점: outlier에 매우 약한 알고리즘 
7. partial clustering: 모든 점들을 cluster에 넣지 않음 (이유: 몇몇 점들은 잘 만들어진 group에 안들어갈 수 있기 때문 -> outlier, noise, unintersting background)
## cluster의 다른 type들 
1. prototype-based: 각각의 cluster는 centroid와 같은 각각의 prototype에게 가까운 object들로 이루어져있음  
2. density-based: 점들의 dense region으로 cluster가 이루어져 있음. -> k-means 쓰면 이상하게 되니까 스면 안됨 
3. graph-based: node는 점들이고 link는 점들 사이의 관계를 나타냄. 그래프에서 connected component로 이루어진 집합인 cluster -> k-means 못 씀. 다른 graph-based 알고리즘 써야함 

## K-means
* 가장 오래되었고 가장 널리 쓰인 clustering 알고리즘
* k-means 알고리즘 기초 
    1. k개의 점들을 initial centroid로 고름 (k는 사용자가 정의하는 파라미터임)
    2. 반복 
        * k 개의 cluster들을 centroid에 가까운 점들로 할당하여 만들어냄 -> ci는 고정되어있고 x를 objective function이 최소화되도록 조절하는 것
        * 각각의 cluster의 centroid를 다시 계산함 -> x는 고정되어있고 objective function을 최소화하도록 ci를 조정 
    3. centroid가 거의 변화하지 않을때까지 반복함 (너무 대용량의 data인 경우 몇개는 빼고 멈춰도됨)
* K-means는 웬만하면 다 수렴함 
    * 이유: 대부분의 수렴은 초기 단계에서 발생함. (약한 조건이 종종 사용됨)(대용량의 data인 경우 1%만 변해도 stop 시키는 경우가 있음)
    * k-means는 local minimum에 빠질 수 있음 -> 다른 initial centroid들이 다른 결과들을 초래할 수 있음 
* 점들을 가장 가까운 centroid에 할당하기 위해 proximity measure를 정해야함
* centroid
    * 다양하게 정의될 수 있음
    * clustering의 목적과 proximity measure에 달려있음
    * ex. cluster 안에 있는 점들의 평균 
        * mean이 아니라 median이어도 됨
        * centroid와 가까운 실제 점으로 정의해도 됨 
* objective function
    * clustering의 목적을 나타냄
    * clustering의 질을 측정함 
    * ex. 가장 가까운 centroid에서 각각의 점들의 거리를 제곱한것을 최소로 하는 clustering이 좋다 
* 예시 ppt 참고 
* ci랑 x는 동시에 조절할 수 없음(이유: search space가 너무 넓어서 오래 걸림) 따로따로 조절-> local minimum에 빠지는 것임 
### 초기 centroid들 고르기 
* k-means에서 중요한 과정 
* 초기점을 눈으로 정하기 어려움 -> 해결방법: 여러번 돌려서 SSE를 최소화하는 cluster을 찾아야함 
1. 해겳방법1: initial centroid를 랜덤하게 정한다 
    * 단점
        * cluster가 좋지 않게 나올 수도 있음 (좋아 보여도 suboptimal한 clustering이 나타날 수 있음)
        * 여러번 돌린 다음에 SSE를 측정하면 시간이 오래 걸린다. (K의 크기, 데이터의 크기가 크면 잘 안돌아감)
        * 아무리 random하게 많이 돌리더라도 local minimum에 빠질 수 있음 
2. 해결방법2: pre-clustering: 일부sample들을 먼저 뽑고 그거 가지고 clustering하기
    * 단점
        * sample이 작을때만 잘 돌아감 (수백~수천개 정도)
        * k가 sample size와 비교해서 작을때만 (sample들 수가 적으니까 k도 막 늘릴 수 없음)    
2. 해결방법3: 가장 먼 점을 선택하기 
    * 첫번째 점을 랜덤하게 뽑고
    * 그 처음점으로부터 가장 먼 점 뽑아서 각각의 cluster의 initial centroid로 잡기 (centroid가 몰려잇는 것을 빙지!) => 잘 퍼져있는 initial centroid들을 얻을 수 있음 
    * 단점 
        * outlier를 선택할 수도 있음 
        * 가장 먼 점을 찾아서 선택하는데 드는 비용이 있음 
3. 해결방법4: K-means++
### K-means++
* 초기화만 K-means와 다름 
1. random하게 첫번째 점을 뽑는다
2. 2~k까지 반복
    * 가장 가까운 centroid로부터의 거리를 각각 계산: d(x)
    * 각 점의 `d(x)^2`에 따라 확률적으로 할당
    * 가중치가 있는 확률에 따라(거리가 멀수록 뽑힐 확률 높음) 새로운 centroid를 뽑음
3. 나머지 부분은 k-means와 동일 
* 예시 ppt 참고 
### k-means의 단점(한계)
* natural한 cluster들(사람이 한눈에 보고 알수 있는 cluster인데 컴퓨터는 바보같이 못찾아냄)을 찾아내기 어려워할 수도 있음 (아래 3가지 경우에)
    1. cluster 사이즈가 다를때 -> 큰 cluster가 깨져버림 (무조건 절대적인 거리만 따지기 때문에 다양한 크기의 cluster들을 구분하지 못함)
    2. cluster들이 다른 밀도(density) 가지고 있을때 -> 밀도가 작은 cluster가 깨져버림
    3. cluster들이 non-globular(꼬불꼬불) 모양일때 -> 두 개의 cluster가 섞여버림 
    * 이유: K-means에서 사용하는 objective function(SSE)가 우리가 찾고자하는 cluster의 종류와 mismatch(맞지 않음) 
        * K-means의 objective function은 globular하고 같은 사이즈와 밀도의 cluster들에 대해서만 잘 clustering함 
* 장점 (강점)
    * 단순하고 다양한 종류의 data type에 대해 적용 가능
    * **`효율적임`**(속도가 빠르다. 여러번 돌려도 큰 부담이 되지 않는다!) -> 교수님이 기억하라고 하심 
* 약점 (단점)
    * non-globular한, 다른 사이즈와 밀도인 cluster들은 다룰 수 없음
    * outlier에 취약함(complete clustering이기 때문에 모든 점을 cluster에 포함시켜야하므로 outlier도 cluster에 포함될 수 있음)
    * center(centroid)라는 중심점이라는 개념이 존재할 수 있는 data에서만 돌아갈 수 있음 (ex. 텍스트 데이터는 중심점이 존재하기 어렵잖니) 

## Agglomerative Hierachical Clustering
* hierachical clustering을 하는 2가지 방법
    1. agglomertaive(응집형): 가까운 거리를 찾는게 나누는 것보다 나음 -> 많이 쓴다 
        * 작은거에서 큰걸로..
    2. divisive(나누는형?): 느림 -> 잘 안씀 
        * 큰거 하나에서 작은걸로 나누는 거임 (나누는 거 찾는게 더 시간이 오래걸리는건 당연하겠죠? 뭘로 어떻게 나누지..해야하니까)
* 기본 알고리즘 
    1. proximity matrix를 계산한다 (필요하다면) (cluster 사이의 거리를 어떻게 정의하느냐에 따라 결과가 완전 달라질 수 있음)
    2. 아래 과정을 하나의 cluster만이 남을떄까지 반복함
        1. 가장 가까운 2개의 cluster를 합침(merge)
        2. 새로운 cluster를 반영하기 위해 proximity matrix를 업데이트 -> matrix 계산 때문에 오버헤드가 커짐 (거리^2로 계산하니까 느려!)
* dendrogram : hierachical clustering은 tree처럼 생긴 diagram인 dendrogram으로 그래프적으로 표현 가능 
    * y축은 뭉친 cluster간의 거리 : 위로 갈수록 더 먼 cluster들끼리 뭉쳐있음 
### cluster 사이의 거리 정의
* hierachical clustering에서의 핵심 : `linkage function` (서로 연결하는 함수)
1. MIN(single link or single linkage): 다른 cluster에서 가장 가까운 두개의 점 사이의 거리, cluster가 쉽게 커짐 
2. MAX(complete link or complete linkage): 다른 cluster 안에서 가장 먼 두개의 점 사이의 거리, cluster가 잘 안커지려고 함 
3. Group Average(average link or average linkage): MIN과 MAX의 중간정도 (먼거리를 반영하기 때문에 MAX와 더 비슷하다) 특성을 가진 방법. 무난하지만 시간이 오래 걸림(모든 점들 사이의 거리를 측정해야함)-> 다른 cluster 사이의 모든 점들의 평균 
4. Ward's method(Ward's link or Ward's linkage): 2개를 뭉쳤을 때 SSE가 얼마나 증가하는지를 봄 -> SSE가 적게 늘어나는 것이 가장 최적일 것이다 
    * 하지만 수학적으로 불안한 알고리즘임 (취하는 행동은 응집형인데 하고자하는 목표는 k-means인점..)
#### MIN(Single Link)
* 장점: 원형이 아닌(옆으로 길쭉한..non-elliptical)한 모형에 좋음
* 단점: noise와 outlier에 취약함 (noise를 먹으면서 뭉쳐질 수도 있음)
#### MAX(Complete link)
* 원형으로 뭉쳐지는 경향이 있음
* 장점: outlier와는 거리가 멀어져서 잘 안붙으려고함 -> outlier 영향 적게 받음 
* 단점: 큰 cluster가 깨질 수도 있음, 거리가 멀어져서 잘 안붙을 수도 있음 



---
## 시험 대비 핵심 정리 
* cluster analysis(clustering)의 목적 2가지 
* classification과 clustering의 차이 
* clustering의 종류 7가지
* cluster의 종류 3가지 

* k-means 장점/단점 
* proximity measure? objective function? 
* 초기 centroid 정하기 4가지 방법 그리고 단점 