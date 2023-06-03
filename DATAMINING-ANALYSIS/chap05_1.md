# CHAP 05_1

## 사전 지식
* Association Analysis
    * 큰 data set들에서 숨겨진 흥미로운 관계를 찾아내는 것 
        1. `frequent itemsets` : transaction들에서 많이 발견되는 set of items
            * (a,b): a와 b가 동시에 많이 등장한다 
        2. `association rules`: 두 개의 itemset 사이의 관계들 
            * a -> b : a를 산 사람 중에 상당히 많은 비율이 b를 산다 
    * 적용 : bioinformatics, medical diagonsis, web mining, scientific data analysis 
    * association analysis의 2가지 key issues 
        1. 원본 데이터가 너무 크므로 계산할 때 비용이 비쌀 수 있음 
            * 조합이 너무 많아서 `효율적 계산`이 어려움 
        2. 발견된 pattern들이 `spurious`할 수 있다 (우연히 발견될 수 있다)
            * 어떤 rule이 가장 흥미로운 것인가? 
            * 모든 rule이 중요한 것은 아님! 
    * 각각의 row는 transaction
    * 각각은 item들 
    * 각각의 transaction ti는 I의 item들의 subset을 포함한다 -> I의 부분집합은 ti다
    * Itemset: 아이템들의 집합 
        * `k-itemset`: k개의 item들로 이루어진 itemset
    * X가 ti의 원소라면, transaction ti는 itemset X를 포함한다. 
* Support Count
    * 우리가 가진 data에서 몇 번 등장하나? 
        * X를 포함하고 있는 transaction의 개수 
    * s(X): support for an itemset X 
        * X가 일어나는 transaction들의 비율 
    * s(X)>=minsup이면 itemset X는 frequent하다 
        * minsup은 사람이 정해야함 
* Association rule 
     1. s(X->Y): X->Y assocation rule의 support -> X랑 Y가 몇 번 등장을 count(support) -> minsup(minimum support)을 넘겨야함 
        * minimum support를 사람이 정해줘야 한다
    2. c(X->Y): X->Y assocation rule의 confidence -> X를 산 사람 중에 몇프로가 Y까지 샀나? -> minmum confidence를 정해줘야함 
* Support와 Confidence를 사용하는 이유 
    1. Support: *일정 수치 이상이 나와야 의미있는 rule임* 
        * 이유 : 비율이 낮다면 이것은 `우연`에 의해 발생한 것이 되기 때문 
    2. confidence: *상당수의 많은 비율이 y를 사야 해당 rule이 의미있는 것* 
        * 이유 : 이것은 rule에 의해 만들어진 추론의 reliability를 측정하기 때문 
    * X -> Y는 X가 있기 때문에 Y를 산 것이 아님 
        * **`원인을 나타내는 것이 아님`** (감히 유추할 수 없다)
        * 그저 같이 나타나는 것 뿐임  (strong co-occurence)
* Brute-Force 접근 : support랑 confidence 모든 가능한 rule에 대하여 계산함
    * 단점: 계산비용이 감당할 수 없을정도로 큼 -> 지수적으로 많은 가능한 rule들이 존재하기 때문 
    * 계산을 최적화하는 방법 : 최소한 minsup, minconf를 넘겨야함 
        1. minsup을 넘기는, 같이 등장하는 frequent itemset을 찾음
        2. 거기서 minconf를 넘기는 itemset을 찾음 
        * X->Y는 XUY가 frequent하지 않으면 frequent하지 않다 => frequent한 itemset들만 고려하자 
* Improved approach : 가장 흔한 association rule mining algorithms 전략 -> 문제를 두가지 주요한 subtask들로 분해한다 
    1. **`frequent itemset generation (단점: 비용이 비쌈)`**
        * minsup을 넘기는 frequent한 itemset들을 찾는다 
        * 쉽지 않음 
        * k개의 item들을 가지는 data set은 최대 `2^k -1` 개의 possible한 itemset들을 만들어냄 
        * k는 실제 적용할때 클 수가 있어서, itemset들의 search space가 지수적으로 커질 수 있음 (possible한 itemset들이 기하급수적으로 커질 수 있음)
        * brute-force 접근
            * 모든 candidate itemset들에 대해 support count를 셈 
                * 각각의 transaction에서 모든 candidate itemset을 체크해봄 
                * 만약 canddiate가 transaction에 포함된다면, support count 증가 
                * 그러나 비쌀 수 있음 
                    * 이유: `O(NMw)`
                        * M은 2^k에 비례함 -> 엄청나게 큰 숫자, transaction들에 대해 M번씩 비교해야함 
                        * w는 transaction에서 가질 수 있는 최대의 item 갯수  
    2. **`rule generation (덜 비쌈)`**
        * frequent itemset들 중 높은 confidence rule을 가지는 것들을 모두 뽑아냄

## Apriori Algorithm
* frequent itemset generation에서 `candidate itemset`을 줄이는데 사용함 
* minsup 이상 등장하는 frequent itemset이라면, 모든 그것의 subset들 또한 frequent 해야한다. 
    * 뒤집으면, *만약에 {a,b}가 frequent하지 않다면 a,b를 포함하는 어떤 애들도 frequent할 수 없음* 
* => 일단 itemset이 frequent하지 않다면, 그것의 superset들을 즉시 prune(버린다)
* 자세한 내용 ppt 참고 
* 효과: k-itemset의 candidate를 오직 frequent한 (k-1) itemsets을 가지고 만들어낼 수 있다. (ppt 참고) => *candidate itemsets의 수를 68%정도 감소시킬 수 있다.*
### 의사코드 
* F1: 하나짜리를 다 count하여 frequent를 찾아내기 
* k= 2, 3, ...
    1. Fk-1을 사용하여 Ck를 생성
        * 조건1: Complete: frequent한 애들이 조합으로 만들어져야함 
        * 조건2: Non-rebundant: 동일한 결과가 나타나지 않도록 해야함 
        * 조건3: Effective: 정말 가능성 있는 후보들만 나오게 해야함 
        1. Fk-1 x F1 방법 : 동일한 아이가 여러 조합으로 만들어질 수 있음 -> 이미 만들었나를 체크해야하는 비용이 듬 
            * Non-rebundant
            * complete: 모든 frequent한 조합은 하나도 안빠지고 나온다 
                * 이유: 모든 frequent한 k-itemset은 frequent한 (k-1)-itemset와 frequent한 1-itemset으로 구성되어 있기 때문 
                * 단점: 많은 unnecessary한 후보들을 만들어낼 수 있음
            * duplicate candidates(중복되는 후보들)이 나옴 
                * 해결방법: 알파벳 순으로 item들을 정렬 후 결합 => 모든 candidate k-itemset은 정확히 한 번만 생성될 수 있게됨 
            * 문제: {a,b,c,d}가 만들어지면 {a,b,d}, {a,c,d}.. 등이 frequent한지 체크해봐야하는 문제가 발생함 -> 그래서 Fk-1 x Fk-1 방법 등장
        2. Fk-1 x Fk-1 방법 
            * frequent한 (k-1)-itemset들을 first k-2 item들이 알파벳 순으로 정렬된 것들이 동일할때만 merge하는 것 
                * complete 
                * duplicate candidate 방지 가능 : 어떤 애가 나올 수 있는 방법은 딱 1가지이기 때문 
                * Fk-1 x F1 방법보다 효과적으로 candidate의 수를 줄일 수 있다. -> 영양가 있는 애들만 모아서 만들기 때문 
    2. 가능성 없는 Ck를 prune함 
    3. Fk를 Ck로부터 결정 
* 더 이상 frequent한 애가 나오지 않을떄까지 k를 증가시키면서 반복함 
### computational cost를 증가시키는 요소
    1. support threshold: minsup이 작을 수록 비용 증가
    2. number of items: item의 수가 많을 수록 candidate itemset이 커지므로 비용 증가
    3. number of transactions: data set에서 repeated pass들을 만드는 비용이 커지므로 비용 증가
    4. average transcation width: transcation 안에 item수가 증가할수록 candidate itemset이 증가하므로 비용 증가 
## maximal frequent itemsets
* transaction dataset이 엄청 커서 frequent itemset이 많이 만들어졌을 때 이것들을 대표하는 compact한 frequent itemset을 뽑자 -> maximal frequent itemsets
* complete함 
* itemset이 엄청 커서 frequent itemset이 지수적으로 엄청 많을 때 valuable한 representation이 됨 
* 그러나, maximal frequent itemset은 그들의 subset에 대한 support 정보를 가지고 있지 않음 

## Evaluation of Association Rules
* association analysis 알고리즘들은 많은 양의 association rule들을 생성해낼 수 있음 
    * 실제 commercial 데이터베이스들의 크기와 차원은 클 수 있어서, 엄청나게 많은 pattern들을 만들어낼 수 있음 -> 근데 대부분의 것들은 not be interesting! 
* => 따라서 rule들의 quality를 평가하는 것이 중요함 -> 객관적인 흥미도 측정 방법을 정의 => interest factor = lift 
### interest factor(lift)
* confidence가 높은 모든 rule들이 흥미로운 것은 아님 
    * 규칙 X -> Y는 X가 Y에 영향을 미치는지 알고자 함 
* 정의: 흥미도(interest factor(lift))는 ..(PPT 참고)
