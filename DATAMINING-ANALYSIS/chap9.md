# CHAP 9

## anomaly
* outlier= anomaly: 대다수의 패턴과 다른 경향을 보이는 것
* 대다수의 데이터가 그리는 pattern, distribution과 다르면 outlier(anomaly)
* 적용 사례: fraud detection, intrusion detection, ecosystem disturbances, medicine and public health, aviation safety
* anomaly는 희귀하게 발생함 
* anomaly detection 스타일 2가지 
    1. descriptive task: 주어진 data에서 누가 이상한지 묘사해줌 (ex. clustering)
    2. predictive task: 새로 들어온 데이터가 anomaly인지 아닌지를 판단 
* anomaly detection vs. classification 
    * anomaly detection을 classification으로 볼 수 있음 (anomaly이다/아니다)
        * 하지만 classification으로 모델링 할 수 없는 이유: anomaly 개수가 극도로 양이 작아서 모델링할 수 없기 때문. (양이 충분하다면 classification으로 모델링 가능)
### anomaly detection 방법 종류 
1. Model-based vs. Model-free
    * Model-based: 패턴을 모델링 -> 모델과 안맞으면 anomaly (발생확률이 낮으면 outlier(anomaly))
    * Model-free: 정상적인 패턴을 모델로 만들기 어려울 때 score를 매김(모든 점에 대해 거리를 구해서 거리가 멀면 outlier)
2. Global vs. Local
3. Label vs. Score 
    * Label: 내부적으로 score해서 어떤것은 normal/ anomaly라고 라벨링해둔 것 (어느 시점에서 끊은것임) (내부적으로 score한다!)
    * Score: 각 점에 대해 anomaly일 확률을 score함 
### anomaly detection 스타일 4가지 
1. statistical appraoches
2. proximity-based approaches
3. clustering-based approaches
    * clustering에서 anomaly일 조건 2가지 
        1. 굉장히 소규모로 이루어진 cluster일 것
        2. 다른 cluster와 거리가 많이 떨어져있을 것 
4. reconstruction-based approaches

## 1. statistical approach
* 과정
    1. 강제로 분포를 가정함 
    2. x를 넣었을 때 확률이 (사용자가 지정해준 기준점보다) 낮으면 anomaly
* 종류
    1. `parameteric model`: 파라미터(mean, variance)가 존재함 -> f(x)에 넣었을때 발생확률이 낮으면 anomaly, 크면 normal
    2. `non-parmetric model`: 파라미터 없음 -> histogram을 그려서 x를 넣었을때 속하는 bin의 빈도를 봄 -> 1/빈도 (역수 취함)이 크면 anomaly, 작으면 normal
        * 문제: 간격을 어떻게 정할 것인가? 
            * 간격이 너무 좁으면: outlier가 너무 많아짐 (다 outlier라고 판별해버릴수도..)
            * 간격이 너무 크면: outlier 범위도 포함되어버림 
* 장점
    * 이론적인 근거가 충분
    * 표준 통계 기법 사용 가능
* 단점 
    * 잘못된 분포 가정을 하면 anomaly 판단 잘못할수도 있음
    * 고차원의 데이터의 경우 적용할 수 있는 통계 기법이 많이 없고, 이마저도 느림 

## 2. Proximity-based approaches
* model-free 
1. `distance-based anomaly score` 
    * dist(x,k)를 구함 
    * normal이면 dist(x,k)가 작고, anomaly이면 dist(x,k)가 크게 나옴 
    * 문제: k 
        * k가 너무 크면: 소규모의 정당한 cluster인데도 anomaly라 판단할 수도 있음
        * k가 너무 작으면 : 소규모로 모여있는 outlier인데도 normal이라 판단해버릴 수도 있음 
        * 완화방법 2가지 
            1. **average** : 1부터 k까지의 dist(x,k)를 구하여 이들의 평균을 anomaly score로 한다
            2. **median**: 마찬가지로 1부터 k까지의 dist(x,k)에서의 중앙값을 anomaly score로 함 
2. `density-based anomaly score`
    * n/V(d)
    * d는 파라미터 
    * 문제: d
        * d가 너무 크면: anomaly도 밀도가 크게 나올 수 있음
        * d가 너무 작으면: normal이 밀도가 작게 나올 수 있음 
    * anomaly score= 1/밀도 (역수 취함)
3. `Relative Density-based anomaly score`
    * 지역적으로 밀도가 다를때 발생하는 문제 해결 
    * 주변의 밀도와 나 자신의 밀도의 비 
    * 상대 밀도 = 주변 밀도들의 평균 / 내 자신의 밀도 
    * 상대 밀도가 크면: 주변 밀도들에 비해서 내 것의 밀도가 작다 -> anomaly
    * 상대 밀도가 작다: 주변 밀도들에 비해서 내 것의 밀도가 크다 -> normal
    * LOF(Local Outlier Factor)를 많이 사용함 
* 장점
    * 어떠한 data 분포에도 적용이 가능
    * 적절한 proximity measure만 주어지면 적용범위가 넓다 
    * 직관적으로 이해하기 쉽다
* 단점
    * *계산적인 비용이 크다* (이유: dist(x,k), 즉 k번째로 먼 것의 거리를 알아야하므로 모든 pair 간의 거리를 측정하여 k번째로 먼 것을 찾으므로 계산 부담이 크다.) -> *시간 복잡도: O(n^2)*
    * *proximity measure가 다르면 결과가 달라짐*
    * 파라미터 (d,k)를 정하는 것이 어려움 

## 3. Clustering-based approaches
1. 클러스터가 소규모고, 클러스터 간의 거리가 멀면 anomaly(outlier)
2. k-means에서 cluster 안에서 centroid간의 거리가 너무 멀면 anomaly(outlier)
* 계층적 클러스터링에서는 98% 정도 merge가 끝나고 나머지 2%가 아직도 merge가 안되어 있으면 이 2%가 outlier
* anomaly score = K-means에서 cluster 내의 centroid와 점 사이의 거리 
* 문제1: 밀도가 지역마다 다를때 절대거리 말고 상대거리(주변애들과의 거리를 보고 그 거리에 대해 내 거리가 어떤지를 봐야함)
    * 상대 거리= centroid와의 거리를 다 측정하고 median을 취함 -> 이 median과 나머지의 비율이 relative distance가 됨 
* 문제2: k-means의 경우 우리의 의도와 다르게 outlier 때문에 cluster가 왜곡될 수 있음 -> 해결:` K-means--` [clustering을 진행하면서 centroid와 멀리 떨어져있는 outlier들을 제거하면서 계속 clustering함]
* 문제3: k -> k가 어떤지에 따라 outlier인지/아닌지의 결과가 달라질 수 있음 
* 장점
    * clustering은 unsupervised라서 알아서 잘 돌아가서 결과를 보여줌
    * clustering 결과도 서비스로 얻을 수 있음
    * k-means를 사용하는 경우 속도가 빠름 
* 단점
    * *cluster의 개수(k)를 모름 -> 얘에 따라서 결과가 많이 달라질 수 있음*
    ** outlier의 존재가 clustering을 많이 왜곡*시킬 수 있음 

## 4. Reconstruction-based approaches
* 원본 데이터를 압축(정보손실 있음) -> 다시 압축을 풀었을때 원본데이터와 얼마나 달라져있는가(reconstruction error가 크면 anomaly)
* `PCA`
    * 단점: linear한 선 밖에 못 그음 -> 너무 단순한 모델 
    * 해결: Autoencoder
* `AutoEncoder`
    * nonlinear하게 선을 찾을 수 있음 -> 복잡한 모델이 됨
    * unsupervised setting이고
    * 목적함수: 넣어준 값과 출력값이 최대한 같도록 해야함 
    * 압축: encoding/ 복원: decoding
    * backpropagation