# CHAP 03 

## 바둑에서 나타나는 모든 경우의수
* 엄청나게 많음 
* 놓을 수 있는 곳 19*19=361 , 놓을 수 있는 모양: 흰돌,검은돌,비워놓기 
    * 3^361 -> 완벽한 탐색 불가능 

## 틱택토 게임 - 미니맥스 알고리즘 
* ppt 참고 

## 미니맥스 알고리즘
* 의사 코드
    ```python
    function minimax(node, depth, maxPlayer) 
    if depth == 0 or node가 단말 노드 then
        return node의 휴리스틱 값
    if maxPlayer then // max node 
        value ← −∞
        for each child of node do
            value ← max(value, minimax(child, depth − 1, FALSE))
        return value
    else //min node 
        value ← +∞
        for each child of node do
            value ← min(value, minimax(child, depth − 1, TRUE))
        return value
    ```
* 분석
    * 완결성 : 완결될 수 있다
    * 최적성: 최적의 알고리즘이다
    * 시간 복잡도: 트리 최대 깊이=m, 분기 개수=b -> `O(b^m)`
    * 공간 복잡도: 똑같이 `O(b^m)`
* 틱택토 구현 (ipynb 파일 참고)

## 알파베타 가지치기(pruning)
* 의사 코드
    ```python
    function alphabeta(node, depth, α, β, maxPlayer)
    if depth == 0 or node가 단말 노드 then
        return node의 휴리스틱 값
    if maxPlayer then // 최대화 경기자
        value ← −∞
        for each child of node do
            value ← max(value, alphabeta(child, depth−1, α, β, FALSE))
        α ← max(α, value)
        if α ≥ β then
            break //이것이 β 컷이다. 
        return value
    else // 최소화 경기자
        value ← +∞
        for each child of node do
            value ← min(value, alphabeta(child, depth−1, α, β, TRUE))
        β ← min(β, value)
        if α ≥ β then
            break //이것이 α 컷이다.
        return value
    ```
* pdf 그림들 참고 

## 불완전한 결정 
* 미니맥스 알고리즘은 탐색 공간 전체를 탐색하는 것을 가정 .. 
    * how? : 탐색을 끝내야 하는 시간에 도달하면 탐색을 중단-> 탐색중인 상태에 대하여 휴리스틱 평가 함수(evaluation function)를 적용 (비단말 노드이지만 단말 노드에 도달한 것처럼 생각)