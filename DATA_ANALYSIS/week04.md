# week 04 

## pandas 라이브러리로 데이터 가공하기 
* dataframe에서 series 추출 가능 
    ```python
    #예시 
    countries=doc['country']
    ```
* series로 feature을 상세하게 탐색하기
    * `size`: 사이즈 반환
    * `count()`: 데이터가 없는 경우를 뺀 사이즈 반환
    * `unique()`: 유일한 값만 반환
    * `value_counts()`: 데이터가 없는 경우를 제외하고 갯수를 반환
* 필요한 column만 선택하기 
    ```python
    covid_stat=doc[['confiremd','deaths','recovered']]
    covid_stat.head()
    ``` 
* 특정 조건에 맞는 row 검색하기 
    ```python
    doc_us=doc[doc['country']=='US'] #country 컬럼에 값이 US인것들만
    ```
* 결측치(NaN) 제거 
    ```python
    doc=pd.read_csv('파일.csv',encoding='utf-8-sig')
    doc.isnull().sum() #각 컬럼별로 몇개의 행에 결측치가 있는지 
    ```
    ```python
    doc=doc.dropna() #결측치 있는 행들 삭제 
    ``` 
    ```python
    doc=doc.dropna(subset=['confirmed']) #confirmed에 NaN이 있는 행들만 삭제 
    ```
    ```python
    doc=doc.fillna(0) #특정값으로 결측치를 대체 (여기선 0으로 바꿈)
    ```
    ```python
    #결측치에 여러 컬럼에서 결측치가 있는 행들의 값을 특정 값으로 대체 
    nan_data={'deaths':0,'recovered':0}
    doc=doc.fillna(nan_data)
    ```
* 특정 키 값을 기준으로 데이터 합치기 
    * `groupby()`: SQL구문의 group by와 동일. 특정 칼럼을 기준으로 그룹
    * `sum()`: 그룹으로 되어있는 데이터 합치기
    ```python
    doc=doc.groupby('country').sum() #국가별로 데이터 합치기 
    ``` 
* 컬럼 타입 변경 
    * `astype({컬럼명: 변경할타입})`: 특정 칼럼의 타입을 변경 
* 데이터 프레임 컬럼명 변경하기
    ```python
    doc.columns=['country_region','confirmed'] #다시 나열해주면 됨 
    ```
* 데이터 프레임에서 중복된 행 확인/제거하기
    * `duplicated()`: 중복 행 확인 
    * `drop_duplicates()`: 중복 행 삭제중복값
        * 특정 칼럼을 기준으로 중복 행 제거: `subset=특정컬럼`
        * 중복된 경우, 처음과 마지막 행 중 어느 행을 남길 것인지 결정
            * 처음: `keep='first'` (디폴트)
            * 처음: `keep='last'` 

## 데이터프레임간 연결/병합해서 데이터 가공하기 
* `concat()`: 두 데이터프레임을 연결해서 하나의 데이터 프레임으로 만들 수 있음
    * 위/아래(axis=0) 또는 왼쪽/오른쪽(axis=1)으로 연결
    * `pd.concat([데이터프레임1,데이터프레임2])`
* `merge()`: 두 데이터프레임 합치기
    * `merge(데이터프레임1,데이터프레임2)`: 두 데이터프레임에 동일한 이름을 가진 컬럼을 기준으로 두 데이터프레임을 합침 
    * `pd.merge(df1,df2)` 
    * `on=기준컬럼명` 을 통해서 기준 컬럼 설정 가능 
    * SQL의 JOIN 기능과 동일 
        * inner: 내부 join 
        * outer: 외부 join 
        * left: left join 
    * `how='inner'`: 디폴트 

## 실제 데이터 전처리하기 (with pandas)
* apply(): apply함수를 사용하여 특정 컬럼값 변경 가능 
실습한 내용은 따로 기록하지 않겠음 (추후 시간되면 기록)