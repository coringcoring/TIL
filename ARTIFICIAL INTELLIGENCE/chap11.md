# CHAP 11

## kNN (k-Nearest Neighbor)
* 모든 기계 학습 알고리즘 중에서도 가장 간단하고 이해하기 쉬운 분류 알고리즘
* k는 홀수로 하는것이 좋음 
* 장점: 어떤 종류의 학습이나 준비시간이 필요 없음. 데이터 크기가 너무 크지 않다면 간단하게 사용 가능
* 단점: 특징 공간에 있는 모든 데이터에 대한 정보가 필요함 
    * 가장 가까운 이웃을 찾기 위해 새로운 데이터에서 기존의 모든 데이터까지의 거리를 확인해야하므로 
    * 데이터와 클래스가 많다면 메모리 공간과 계산 시간이 많이 필요 
### 시험 대비 코드 정리 
```python
scaler=StandardScaler()
scaler.fit(iris.data)
data_std=scaler.transform(iris.data)
```
```python
knn=KNeighborsClassifier()
knn.fit(X_train,y_train)

y_pred=knn.predict(X_test)
scores=metrics.accuracy_score(y_test,y_pred)
```
```python
plt.imshow(digits.images[7],cmap=plt.cm.gray_r,interpolation='nearest')
```
```python
y_pred=knn.predict([X_test[7]]) #입력은 항상 2차원 배열이여야 함
```
```python
conf_mat=confusion_matrix(y_pred,y_test)
```

## K-means 클러스터링
* 비지도학습에서 가장 대표적인 것 
* 주어진 n개의 관측값(데이터)을 k개의 클러스터로 분할하는 알고리즘. 관측값들은 거리가 최소인 클러스터로 분류됨 
### 시험 대비 코드 정리 
```python
kmeans=KMeans(n_clusters=2)
kmeans.fit(X)
print(kmeans.cluster_centers_)
```
```python
score=[kmeans[i].fit(X).inertia_ for i in range(len(kmeans))]
#inertia_: 모든 cluster의 sum of squares 합 값이 나옴 
```

## 의사 결정 트리(DT: Decision Trees)
* 지도학습방법
* 회귀, 분류, 다중 출력까지 가능 
* 장점
    * 인간의 사고를 모방하여서 이해와 해석이 쉬움
    * 숫자, 범주형 데이터 모두 다룰 수 있음
    * 데이터를 분류하는 논리의 흐름을 시각적으로 볼 수 있음
* 단점: 지나치게 복잡한 트리가 만들어질 수 있음
* 탐욕적인 알고리즘: 분리된 결과의 불순도가 낮아지면 변별력이 높은 질문
* 불순도 지표
    1. 엔트로피 : ID3 알고리즘
        * 엔트로피의 최댓값? : 예를 들어 [40,40,40] 이렇게 두고 엔트로피 계산하면 이게 최댓값임 
    2. 지니계수: CART 알고리즘 
### 시험 대비 코드 정리
```Python
clf=tree.DecisionTreeClassifier(criterion='entropy') #디폴트는 gini계수임 
```