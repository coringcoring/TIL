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