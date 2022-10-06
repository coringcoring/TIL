# week 02 

## 01. plaintext file format 

### 파일 오픈 plain text  
* `plain text`: 파일 형식 없이 문자열만 저장된 것. 
* 프로그래밍에서 파일은 다음과 같이 3가지 명령의 순서로 처리 
    1. 파일 오픈
    2. 파일 읽기 또는 쓰기
    3. 파일 닫기 
* 파일디스크립터변수 = open(파일이름,파일열기모드)
```python
data_file = open('00_data/text_data.txt','r',encoding='utf-8-sig')
``` 
* 파일디스크립터 file descriptor 변수: 오픈한 파일 객체를 가리키는 변수
* 파일 이름 명시시, open 함수 실행 위치와 파일이름이 저장된 위치를 파일 절대 경로 또는 상대 경로로 명시해야함.
* 파일 열기 모드 : `r (읽기모드), w(쓰기모드),a(추가모드)`
* encoding: 파일 오픈할 때 해당 파일의 인코딩 방식을 지정해주는 것. 

### 파일 닫기 plain text 
* 파일디스크립터변수.close()로 닫음
* 항상 오픈한 파일은 닫아야함! (파일을 오픈한채로 두면 관련 자원을 계속 사용중이게됨.)
```python
data_file = open('00_data/text_data.txt','r',encoding='utf-8-sig')
data_file.close()
```
* 자동으로 파일을 닫을 수 있음
    * with open() 명령 as 파일디스크립터:
    * with 구문 안에서 동작할 코드를 탭으로 들여쓰기 해서 사용하면 with 구문 끝난 후 자동으로 해당 파일 닫아줌. 
```python
with open('00_data/text_data.txt','r',encoding='utf-8-sig') as file_desc:
    print('test')
``` 

### 파일 읽기 plain text
* readlines() 사용: 오픈한파일디스크립터.readlines() 사용해서 전체 데이터를 한줄씩 리스트타입으로 읽기 가능 
```python
data_file = open('00_data/text_data.txt','r',encoding='utf-8-sig')
data_lines=data_file.readlines()
data_lines
```
 
* readline() 사용: 오픈한파일디스크립터.readline() 호출로 현재까지 읽은 파일 데이터의 다음 한 줄을 문자열 타입으로 읽기 가능 
```python
data_file = open('00_data/text_data.txt','r',encoding='utf-8-sig')
data_line=data_file.readline()
print(data_line)
``` 

```python
1번째 줄을 읽어옵니다
```

```python
data_line=data_file.readline()
```

```python
2번째 줄을 읽어옵니다
``` 

* read() 함수 사용 : 오픈한 파일디스크립터.read() 함수를 호출해서 전체 파일 데이터를 문자열 타입으로 읽을 수 있음 
```python
data_file = open('00_data/text_data.txt','r',encoding='utf-8-sig')
data=data_file.read()
data
```
```python
1번째 줄입니다. 2번째 줄입니다.
```

### 파일 쓰기 plain text
* write() 함수 사용하기 : open() 함수의 파일열기모드를 'w'로 해서 파일 쓰기 
```python
data_file = open('00_data/text_data.txt','w',encoding='utf-8-sig')
data_file.write("안녕하세요")
data_file.write("김진영입니다")
data_file.close() 
```

라인을 바꿔서 쓰려면.. 
```python
data_file = open('00_data/text_data.txt','w',encoding='utf-8-sig')
data_file.write("안녕하세요\n")
data_file.write("김진영입니다.\n")
data_file.close()
```
* 기존 파일에 데이터 추가: 'a'모드를 사용해야함. 
```python
data_file = open('00_data/text_data.txt','a',encoding='utf-8-sig')
data_file.write("추가합니다")
data_file.close()
```

### csv 포맷 이해 
* csv(Comma-Separated Values): 스프레드시트 데이터를 저장할 때 가장 널리 쓰이는 형식 
* plain text처럼 파일을 open.
* csv 라이브러리 이용 
```python
import csv
```

* `csv.reader(오픈한 파일디스크립터,delimiter=',')`
    * delimiter = 데이터구분자: csv 파일내에 데이터 구분자를 명시할 수 있음(옵션)
    ```python
    import csv
    data_file=open('00_data/USvideos.csv','r',encoding='utf-8-sig')
    data_lines=csv.reader(data_file,delimiter=',')
    ```
    * [] 리스트 형태로 변환해줌. 
* 데이터 읽기 : 각 line별 데이터를 읽기 위해 for문 사용 가능. 
```python
data_lines=csv.reader(data_file,delimiter=',')
for data_line in data_lines:
    print(data_line)
```
* 파일 닫기(close) :plain text처럼 close를 통해 닫아주면 됨. 
```python
data_file.close()
```

### csv 파일 쓰기 
* open시 'w'로 옵션을 설정 
* `newline=''`이라는 옵션을 통해 윈도우 같은 경우에만 csv 모듈에서 데이터를 쓸 때 각 라인 뒤에 빈 라인이 추가되는 문제가 발생. 
* csv.reader 대신에 `csv.writer` 사용
```python
import csv
data_file=open('00_data/tmp_csv.csv','w',encoding='utf-8-sig',newline='')
data_write=csv.writer(data_file,delimiter=',')
data_write.writerow(['1','2','3'])
data_file.close()
```

* with 구문 사용 가능 : 내부 구문 실행 완료 후 자동으로 파일 닫기 
```python
import csv
with open('00_data/tmp_csv.csv','w',encoding='utf-8-sig',newline='') as writer_csv:
    writer=csv.writer(writer_csv,delimiter=',')
    writer.writerow(['love']*3+['banana'])
    writer.writerow(['apple'],2) #문자열 말고도 다른 type 데이터 입력 쓰기 가능 
    writer.writerow(['apps','lovely apps','wonderful apps'])
```

### csv 파일 쓰기 (다른 기법: 사전 타입으로 쓰기)
* `csv.DictWriter` 사용. 
* field 이름 선언 후 데이터 넣기 
* 이 또한 사전 타입으로 읽기(출력) 가능하다. (for문 이용해서)
```python
import csv

with open('00_data/tmp_csv.csv','w',encoding='utf-8-sig',newline='') as writer_csv:
    field_name_list=['first','last'] #필드명 정의 
    writer=csv.DictWriter(writer_csv,fieldnames=field_name_list)
    writer.writeheader() #선언된 필드명을 writeheader() 함수로 넣을 수 있음. 
    writer.writerow({'first':'jin young','last':'kim'})
    writer.writerow({'first':'ji soo','last':'oh'})
    writer.writerow({'first':'su hyun','last':'kim'})
```

### xml 포맷 이해하기
* 데이터를 특정 목적에 따라 태그로 감싸서 마크업하는 범용적인 포맷  
* xml 기본 구조 
```xml
<태그 속성="속성값">내용</태그>
```
* 다른 요소와 그룹으로도 묶기 가능. 데이터를 csv, plain text와 달리 `구조화`가능. 
```xml
<products type="전자제품">
    <product id="001" price="30000">32인치 모니터</product>
    <product id="002" price="21000">24인치 모니터</product>
</products>
``` 

### xml 파일 읽고 데이터 추출
1. open() 함수로 xml 데이터 읽어오기
2. xml 데이터 파싱
3. select()로 원하는 데이터 태그 선택 
    * select()는 항상 return type이 `list`.
```python
data_file=open('users.xml','r',encoding='utf-8-sig') #1.
soup=beautifulSoup(data_file,'xml') #2.
users=soup.select('user') #3. 
for user in users: #4. 리스트니까 for문으로 아이템 추출 
    print(user.text) #5. 원하는 데이터 출력 
```

* parsing 과 데이터 추출 
```python
from bs4 import BeautifulSoup 
soup = BeautifulSoup(xml파일디스크립터,'xml')
soup.select(원하는 데이터 태그)
``` 
* if 이름과 나이 각각 출력하려면?
```python
from bs4 import BeautifulSoup

data_file=open('00_data/users.xml','r',encoding='utf-8-sig')
soup=BeautifulSoup(data_file,'xml')

users=soup.select('user')

for user in users:
    print("이름: ",user.select_one('name').text)
    print("나이: ",user.select_one('age').text)
``` 


### json 포맷 이해 
* JavaScript Object Notation 줄임말
* 서버와 클라이언트 또는 컴퓨터/프로그램 사이에 데이터를 주고받을 때 사용하는 데이터 포맷 
* json 포맷 예: 
    ```javascript
    {'id':'01','language':'java','edition':'third'}
    ```

### json 데이터 포맷 읽기
* json 라이브러리 제공 
* `json.loads()`로 문자열로 된 json 데이터를 사전처럼 다룰 수 있음.
```python
import json 

data={"id":"01","language":"Java","edition":"third","author":"Herbert Schildt"}

jsondata=json.loads(data)
print(jsondata['id'],jsondata['language'],jsondata['edition'],jsondata['author'],type(jsondata))
``` 

* `json.dumps()`로 파이썬 사전 데이터를 json 문자열 데이터로 변환 가능
```python
import json
data={"id":"01","language":"Java","edition":"third","author":"Herbert Schildt"}

jsondata=json.dumps(data) #사전 데이터가 통째로 문자열로 바뀜. 
print(jsondata,type(jsondata)) #문자열로 출력이 됨. 
jsondata=json.dumps(data,indent=2) #2번 들여쓰기 
print(jsondata,type(jsondata)) #2번 들여쓰기 되어 출력되는 것을 확인 가능. 
``` 

* `json.dump()`로 파이썬 사전 데이터를 파일로 쓰기 가능 
```python
import json
data={"id":"01","language":"Java","edition":"third","author":"Herbert Schildt"}
data['language']=['Java','C']

with open('00_data/test.json','w',encoding='utf-8-sig') as json_file:
    json_string = json.dump(data,json_file,indent=2)
``` 

* `json.load()`로 파일로 된 json 데이터를 사전처럼 다루기 가능 
    ```python
    import json

    with open('00_data/US_category_id.json','r',encoding='utf-8-sig') as json_file:
        json_data=json.load(json_file) #사전 형태로 json_data에 넣어줌. 
        print(json_data.keys())
    ```
    * items 데이터만 뽑기 (json 데이터에 `리스트`도 포함될 수 있음.)
        ```python
        import json
        with open('00_data/US_category_id.json','r',encoding='utf-8-sig') as json_file:
            json_data=json.load(json_file)
            for item in json_data['items']:
                print(item['kind'],item['snippet']['title'])
        ```