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
    * 좋은 clustering
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
2. density-based: 점들의 dense region으로 cluster가 이루어져 있음. 
3. graph-based: node는 점들이고 link는 점들 사이의 관계를 나타냄. 그래프에서 connected component로 이루어진 집합인 cluster 

## K-means
* 가장 오래되었고 가장 널리 쓰인 clustering 알고리즘
* k-means 알고리즘 기초 
    1. k개의 점들을 initial centroid로 고름 (k는 사용자가 정의하는 파라미터임)
    2. 반복 
        * k 개의 cluster들을 centroid에 가까운 점들로 할당하여 만들어냄
        * 각각의 cluster의 centroid를 다시 계산함 
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
