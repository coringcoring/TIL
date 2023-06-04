# CHAP 12 

* 유닛 = 노드 
* 신경망의 장점 
* 신경망의 장점 
    1. 학습이 가능 
    2. 몇 개의 소자가 오동작을 일으키더라도 전체적으로는 큰 문제가 발생하지 않음 
* 퍼셉트론 학습 알고리즘 (PPT 참고)
```Python
    for t in range(epochs):
        print("epoch=", t, "======================")
        for i in range(len(X)):
            predict = step_func(np.dot(X[i], W)) #np.dot: X 벡터와 W 벡터를 내적 #predict 값은 0 또는 1이 나옴  
            error = Y[i] - predict		# 오차 계산, y[i]는 정답값, predict는 예측값
            W += eta * error * X[i]		# 가중치 업데이트
            print("현재 처리 입력=", X[i], "정답=", Y[i], "출력=", predict, "변경된 가중치=", W)
``` -> 핵심 코드가 보관중 