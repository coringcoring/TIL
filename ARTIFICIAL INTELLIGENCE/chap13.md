# CHAP 13

### 신경망의 종류 
* 전방신경망, 순환신경망
* 얕은 신경망, 깊은 신경망
* 결정론 신경망: 계산식에 임의성이 전혀 없기 때문에 입력이 같으면 항상 같은 출력
* 스토캐스틱 신경망: 계산식이 확률에 따른 난수 사용하므로 입력이 같아도 매번 다른 출력이 나옴 

## 퍼셉트론
* 2차원: 결정 직선, 3차원: 결정 평면, 4차원: 결정 초평면 (hyperplane)

## 다층 퍼셉트론
1. 은닉층을 둔다: 더 유리한 새로운 특징 공간으로 변환 가능 
2. 시그모이드 활성함수 도입 : 연성 의사결정 가능 
3. 오류 역전파 알고리즘 사용 
* 퍼셉트론을 병렬로 결합 -> 새로운 특징공간 z에서 선형 분리가 가능해짐 
    * 여기에 퍼셉트론 1개를 순차 결합 -> 다층 퍼셉트론 
* 하이퍼 파라미터 : p 개의 은닉 노드 
    * p가 너무 크면 overfitting
    * p가 너무 작으면 underfitting
* 입력 노드: d+1 개 
* 출력 노드: c개 
* 은닉층은 특정 벡터를 분류에 더 유리한 특징 공간으로 변환함 -> 특징 학습 (feature learning)

## 오류 역전파 알고리즘 

## 미니배치 VS 스토캐스틱 경사 하강법
* 미니배치는 그래디언트의 잡음을 줄여주어서 빠른 수렴이 가능함 -> 표준적으로 많이 쓰임 
* 평균 그레이디언트로 갱신(PPT 참고)

* 다층 퍼셉트론에서 은닉층을 하나만 가졌을때, 은닉노드가 충분히 많다면 어떤 함수든 모두 근사화할 수 있다 (범용근사자, Universal approximator) (그러나 실질적으로 은닉 노드가 무한개일 수는 없으므로 한계가 있음)
* 순수한 최적화 알고리즘으로는 높은 성능 불가능
    * 이유: 데이터 희소성, 잡음, 미숙한 신경망 구조
* 휴리스틱 개발 쟁점 4가지 
    1. 아키텍처: 은닉층, 은닉 노드 개수 정하기
    2. 초깃값: 가중치 초기화 
    3. 학습률 : 학습률은 얼마정도로?  
    4. 활성함수: 어떤 활성함수를 사용할 것인가? 

## MLP 구현 (핵심 코드 위주로 정리)
```Python
# 시그모이드 함수
def actf(x):
	return 1/(1+np.exp(-x))

# 시그모이드 함수의 미분치, 시그모이드 함수 출력값을 입력으로 받는다. 
def actf_deriv(out):
	    return out*(1-out)
```
```python
# 순방향 전파 계산
def predict(x):
        layer0 = x			# 입력을 layer0에 대입한다. 
        Z1 = np.dot(layer0, W1)+B1	# 행렬의 곱을 계산한다. 
        layer1 = actf(Z1)		# 활성화 함수를 적용한다. 
        Z2 = np.dot(layer1, W2)+B2	# 행렬의 곱을 계산한다. 
        layer2 = actf(Z2)		# 활성화 함수를 적용한다. 
        return layer0, layer1, layer2
```
```python
# 역방향 전파 계산
def fit():
    global W1, W2, B1, B2		# 우리는 외부에 정의된 변수를 변경해야 한다. 
    for i in range(90000):		# 9만번 반복한다. 
        for x, y in zip(X, T):		# 학습 샘플을 하나씩 꺼낸다. 
            x = np.reshape(x, (1, -1))	# 2차원 행렬로 만든다.
            y = np.reshape(y, (1, -1))	# 2차원 행렬로 만든다. 

            layer0, layer1, layer2 = predict(x)			# 순방향 계산
            layer2_error = layer2-y				# 오차 계산
            layer2_delta = layer2_error*actf_deriv(layer2)	# 출력층의 델타 계산 
            layer1_error = np.dot(layer2_delta, W2.T)		# 은닉층의 오차 계산
            layer1_delta = layer1_error*actf_deriv(layer1)	# 은닉층의 델타 계산
            
            W2 += -learning_rate*np.dot(layer1.T, layer2_delta)	#
            W1 += -learning_rate*np.dot(layer0.T, layer1_delta)	# 
            B2 += -learning_rate*np.sum(layer2_delta, axis=0)	#
            B1 += -learning_rate*np.sum(layer1_delta, axis=0)	# 
```

