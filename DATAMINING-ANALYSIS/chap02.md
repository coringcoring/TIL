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

## 데이터 셋의 특징 
* 데이터를 받으면 데이터의 차원 개수가 어떤지 확인하고, 분포가 잘 되어 있는지 확인 
* 차원:
    * 차원의 개수를 줄여야함. 
    * 차원의 저주 
* Distribution
    * 특정 값들이 한쪽으로 편향되어 있는지? 