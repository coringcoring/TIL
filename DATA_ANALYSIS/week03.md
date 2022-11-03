# week 03 

## 1. pandas 

### pandas란
* 테이블형 데이터를 다룰 수 있는 다양한 기능을 가진 라이브러리 
* `raw data`를 데이터 분석 전과정을 위해 사용할 수 있도록 변환하는 데이터 전처리에도 많이 사용됨 
* import 
    ```python
    import pandas as pd
    ```

### Series 생성
* 데이터를 다루기 위해서 dataframe, series 제공 
* series는 1차원 데이터. dataframe은 테이블형(2차원) 데이터
* Create
    ```python
    seriesdata=pd.Series([70,60,90])
    ``` 

    ```python
    0 70
    1 60
    2 90
    dtype: int64
    ```
* index는 행의 레이블. index를 지정하지 않으면 0부터 시작하는 인덱스 자동 생성
    ```python
    seriesdata=pd.Series([70,60,90],index=['국어','음악','체육'])
    ```

### Series 데이터 읽고 수정하기 (Read & Update)
* 읽기
    ```python
    print(seriesdata['미술'],seriesdata[0])
    ```
* update 하기 
    ```python
    seriesdata['미술']=80
    print(seriesdata['미술'],seriesdata[0])
    ```

### Series 데이터 삭제하기 (Delete)
* 삭제 
    ```python
    del seriesdata['미술']
    ``` 

### pandas 데이터 타입 
* 파이썬과 다름. 
    * object는 파이썬의 str 또는 혼용 데이터 타입(문자열)
    * int64는 파이썬의 int(정수)
    * float64는 파이썬의 float(부동소수점)
    * bool은 파이썬의 bool
    * 그외: datetime64(날짜/시간),timedelta(두 datatime64간의 차이)
* 변경 
    ```python
    seriesdata.astype('float')
    ``` 

### 데이터프레임 dataframe 생성 Create
* 생성
    ```python
    import pandas as pd

    df=pd.DataFrame({
        "미국":[2.1,2.2,2.3],
        "한국":[0.4,0.5,0.45],
        "중국":[10,13,15],
        index=[2000,2010,2020]
    })
    ```

## 데이터프레임 Read, Update 
* Series는 index와 values
    ```python
    df.index #행방향 index
    ```
    ```python
    df.columns #열방향 index
    ```
    ```python
    df.values
    ``` 
* 인덱스로 특정 컬럼 선택하기 
    ```python
    df=pd.DataFrame({
        "년도":[2000,2010,2020], 
        "미국":[2.1,2.2,2.3],
        "한국":[0.4,0.5,0.45],
        "중국":[10,13,15]
    })

    df=df.set_index('년도')
    df=df.reset_index('년도')
    ```
* 데이터프레임 접근하기 
    *  특정 행 가져오기
        ```python
        df.loc[2000]
        df.iloc[0]
        ``` 
    * 특정 열 가져오기 
        ```python
        df(['미국'])
        print(df['미국'][2000])
        print(df.loc[2000]['미국'])
        ``` 
* 특정 행 추가하기
    ```python
    df.loc[2021]=[1,2,3]
    ``` 
* 행 삭제 
    ```python
    df.drop(['2020'])
    ```
* dataframe 컬럼 선택(복사): 원본데이터는 놔두고, 복사해서 데이터 처리. 컬럼 리스트를 넣는다고 생각. 
    ```python
    df2=df[['중국','한국']].copy()
    ```

### 탐색적 데이터 분석의 이해 
* 탐색적 데이터 분석 과정
    * EDA(Exploratory Data Analysis)
    * 데이터 분석을 위해 raw data를 다양한 각도에서 관찰하여, 데이터를 이해하는 과정 
        1. 데이터의 출처와 주제에 대해 이해
        2. 데이터의 크기 확인
        3. 데이터의 구성 요소의 속성 확인 