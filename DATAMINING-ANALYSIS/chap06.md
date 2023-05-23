# CHAP 06

* Ch5까지에서는 이런 가정들을 가지고 association rule mining을 했었음
    * input 데이터는 items이라 불리는 binary attribute들로 구성되어 있음
    * transaction 안에 item이 나타나는가(presence)가 더 중요함 (absence보다)
    * -> item은 0보다 1이 더 중요하게 다뤄짐 (asymmetric binary attribute) , frequent한 패턴들만이 흥미롭게 다뤄짐 

## Handling Categorical Attributes
* pattern을 추출하기 위해 categorical attribute들은 'items'으로 변형시켜야함
* 각각의 attribute-value pair의 새로운 item을 만들어냄 
### 고려해야할 issue들 3가지 
1. *몇 attribute 값들은 frequent하지 않을 수 있다* 
    * frequent pattern의 part가 되기에 충분하지 않을 수 있음 
    * 해결방법 1: 서로 관련된 attribute value들을 그룹으로 만들어 category로 만들기 -> support를 올릴 수 있음 
    * 해결방법 2: 적게 발생한 value들만 모아서 하나의 single category로 만들기 
    * => minsup을 넘기도록 하여 rule을 만들어내자 
2. *몇몇 attribute value들은 너무 frequent하다* 
    * 의미가 없음
    * 이유: 높은-frequency item들은 어떠한 새로운 information을 이끌어내지 못함 
    * 해결방법: association analysis를 적용하기 전에 이러한 item들을 제거함 
        * pattern을 이해하기에 더 유용할 수 있음 
3. *의미없는 candidate itemset들이 있을 수 있음* 
    * support count가 0이 되므로 영양가가 없음 (동시 만족이 안되기 떄문에!)
    * 해결방법: 똑같은 attribute에 대해서 한 개보다 더 많은 item을 포함하는 candidate itemset이 생성되지 않도록 함 

## Handling Continuous Attributes
* continuous attribute의 인접한 값들을 그룹화하여 유한한 개수의 interval들로 만듦 -> discretization 기술들 사용 가능 
    * equal interval width, equal frequency or clustering
* interval의 개수 
    * 구간을 얼마나 잘게 나누느냐가 어려움 
        * 너무 잘게 자르면 support가 작아져서 rule이 발생하기 어려움 
        * 너무 크게 자르면 support는 충족되었지만 confidence는 맞추기 어려움 
    * key parameter: attribute discretization <- 사용자에 의해서 주어지는 parameter임 
        * equal interval (폭을 동일하게) width approach -> interval width 
        * equal frequency approach -> interval마다 transcation의 개수 
        * clustering-based approach -> cluster의 개수 
### 고려해야할 issue들 3가지 
1. *interval이 너무 wide하면 confidence가 부족하여 몇몇개의 pattern들을 잃을 수 있다* 
    * support는 충족 가능하나 confidence가 low (minconf 못 넘김)
    * confidence를 높이는 게 쉽지 않아서 rule을 찾아내기 어려움 
2. *interval이 너무 narrow하면 support가 부족하여 몇몇개의 pattern들을 찾기 어려움* 
    * minsup을 넘기기 어려움 
3. *interval이 8년, 즉 중간정도 (애매한) 숫자일 경우 rule을 건질 수도 있고 아닐수도 있음* 

## Handling a Concept Hierachy
* a concept hierachy: 특정한 domain에서 정의된 다양한 entity들 또는 개념들의 multilevel organization 
    * leaf node가 평소 우리가 접하는 data의 형태 : minconf는 만족 가능, minsup은 만족 어려움
    * 너무 위의 rule들은 minconf를 만족하지 못할 수도 있음, minsup은 가능 
### concept hierachies를 사용하면 장점 
1. 낮은 level에 있는 items들은 frequent한 itemset에서 나타날 수 있는 충분한 support를 만족 못 할 수도 있음 -> support가 충분하지 않아 rule을 찾아내기 어려움 (그렇다고 minsup을 낳추면 쓸데없는 rule들이 나올 수 있음)
    * concept hieraachy를 사용하지 않으면 different level들에 있는 흥미로운 pattern들을 놓칠 수 있는 가능성이 존재 
2. 낮은 level에서 발견된 rule들은 overly specific한 경향이 있다. 너무나 detail한 rule을 찾을 수도 있음. 그리고 higher level들에서의 rule들만큼 흥미롭지 않을 수 있음 
    * concept hierachy를 사용하면 하나의 single rule로 요약될 수 있음 
3. top level의 item들만 고려하는 것은 좋지 않다. 
    * 이유: 도움이 안되는 rule일 수 있음, 과대한 일반화(overgeneralizes)임 
### extending standard association analysis
* standard association analysis를 확장 가능 
* ppt 참고 
* 이러한 접근은 다른 level들의 rule을 찾아낼 수 있음 
### extension의 한계 
1. 높은 level에 있는 item들은 높은 support count들을 lower level보다 많이 가진다. 
    * 너무 높은 minsup이라면: 위의 item들이 발견될 것임
    * 너무 낮은 minsup이라면: 너무 자잘한 rule들이 발견될 것임
2. concept hierachy는 association analysis algorithms의 계산시간을 늘림 
    * 이유: 많은 수의 item들과 넓은 transaction들 때문에 
    * candidate pattern들과 frequent pattern들은 지수적으로 넓은 transaction과 함께 증가할 것이다 
3. reubundant rule을 생산해낼 수 있다 

## Sequential Patterns
* Market basket 데이터는 temporal 정보를 포함함 (언제 item이 구입되었는지)
    * sequence of transactions
* event-based 데이터는 타고난 sequential nature을 가지고 있음
    * ex. 과학적인 실험에 의해 모아진 data 
* 그러나 지금까지 공부한 association rule들은 오직 `co-occurrence`(같이 발생한 것)에만 집중해왔고 데이터의 sequential 정보는 무시해왔었음 -> 이제 합시다 
* input: a sequence data set 
    * 각각의 행: 주어진 시간에 발생한 event들의 기록 
    * timestamp information은 association analysis의 다른 스타일을 가능하게 함 
    * 어떤 사람(object)이 언제(timestamp) 무엇(events)을 샀나 
* 반드시 전처리 필요! -> 그래야 알고리즘(ex. Apriori) 적용이 가능함 
* output: object들 사이에 sequential order로 흔히 발생하는 event들의 association pattern들 
### sequences 
* elements(transactions)의 ordered list 
* events(items)
* ppt 참고 
* k-sequence: k개의 event들(item들)을 포함하는 하나의 sequence (transcation이 아님!)
### subsequences
* ppt 참고.. 
### sequential pattern discovery 
* data sequences: 하나의 single object와 연관된 elements들로 이루어진 an ordered list 
* ppt 참고 
### challenges in sequential pattern discovery
* 모든 가능한 sequence들은 지수적으로 커서 enumerate하기 어렵다 
    * 가능한 sequence들이 많은 이유
        1. 똑같은게 2번 나올 수 있으므로
        2. itemset은 순서가 중요하지 않았으나, 여기서는 순서가 다르면 다른 sequence임 -> 순서가 중요함 
        3. 조합이 무한대 가능 (ex. in이 무한대 등장해도 됨)
            * itemset에서 가능한 itemset 수는 2^n - 1 이었음 
            * 그러나 여기서는 possible한 sequence들이 무한대 
        => candidate가 엄청 많아짐 
### Apriori Principle for Sequential Data
* Apriori principle을 그대로 쓰되, 딱 1가지만 바꾸면 됨 -> Apriori-like 알고리즘 
    * 후보 k-sequences들을 frequent한 (k-1)-sequence들로부터 만들어냄 (ppt 참고)