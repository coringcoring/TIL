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
