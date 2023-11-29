# Schema Refinement and Normal Forms

## The evils of redundancy(중복성)
* 교수님 왈 중복성은 악마(??)
* relational model에 대한 한정적인 이야기 
    * relation들은 column들로 구성되어 있음 
* 중복(redundancy)를 나타내는 이론 -> 함수(적) 종속성(FDs)
* `decomposition`: 원래 테이블에서 중복 없는 테이블로 나누는 것 
    * 재설계한 테이블들이 사용자가 원하는 수준에서 데이터 중복이 없을때까지 계속함 
### functional dependencies(함수(적) 종속성) (FDs)
* R: x -> y 라는 관계(relation)
    * 함수 개념이 그대로 들어가 있음
        * 함수 개념: 똑같은 x 값이라면 서로 다른 y 값으로 매핑 될 수가 없는 관계 (x가 같으면 y도 같음)
        * table에 나타나는 동일한 record에 대해서 x가 똑같으면 y도 똑같음 (1000개 나오면 1000개 똑같이 나옴)
* *X에서 Y에 대한 함수적 종속성이 있다* = **X가 Y를 함수적으로 결정한다** (X -> Y)
    * 항상 어느 경로에서든지 어떻게 운영이 되든지 임의의 튜플(Record) t1, t2를 뽑았다고 했을때
    * 두 개의 레코드(t1,t2)의 (t1, t2 각각 project한) X값이 같으면 =>(implies) t1,t2의 (t1,t2 각각 project한) Y 값도 똑같다 
### EXAMPLE
* key는 ssn : **S -> SNLRWH** 
    * 함수적 종속성이 존재한다고 볼 수 있나? : YES
        * 모든 키에 대해서 키는 테이블을 함수적으로 지정함 
        * 키 값은 어차피 주어진 테이블에서 한번씩만 나타나므로 
* rating determines hrly_wages : **R -> W**로 가는 함수적 종속성이 존재 
    * 정보의 중복이 나타나서 좋지 않음 (WHY? -> anomaly 발생)
    * Update anomaly: update를 잘못하면 database가 쓰레기 data로 이루어진 db가 될 수 있음 (consistency를 유지하기 어려워짐 -> `data inconsistency`)
        * 만약 R를 5에서 8로 바꾼다면
        * 다른 것들도 다 5에서 8로 바꿔야하는데
        * 정보 불일치가 일어날 수 있음 
        * 따라서 W를 빼고 R과 W에 대한 테이블을 따로 빼면 해결이 된다 (정보 불일치 문제가 발생하지 않음. R에 대한 정보만 테이블에 저장해두기 -> Hourly_Emps2와 Wages 테이블들 참고)
    * insertion anomaly
    * deletion anomaly
        * Smethurst와 Guldu를 삭제한다면 -> R이 5이면 W는 7이라는 정보가 아예 삭제됨 (직원 정보에 R, W를 저장해뒀기 때문) 
        * 만약 새로운 직원(R이 5)이 들어왔는데 R이 5라면, W가 7이어야한다는 정보(부가적인 정보)가 없어서 문제가 발생 
        * 따라서 위에서 해결한 방법처럼 따로 R,W에 대한 정보를 담은 테이블을 따로 빼두자 (Hourly_Emps2와 Wages의 두 테이블로)
### reasoning about FDs
* 논리적 시스템에 의해 확장되면서 더이상 아무리 해도 확장이 안됨 -> 최대 확장치 -> `closure of F` 를 만들어가야한다 
    * F closure을 만들어내는 논리적 시스템이 존재함 
    * F closure가 이상치가 되는 것
* Armstrong's Axioms: 아래 3가지의 논리적 시스템으로 하면 `sound`하고 `complete`한 set이 만들어진다 
    1. Reflexivity
    2. Augmentation
    3. Transitivity 
    