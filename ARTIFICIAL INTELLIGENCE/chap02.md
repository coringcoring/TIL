# CHAP 02 

* 탐색 != 탐방
    * 탐색: 목표치를 찾으면 탐색을 종료함
    * 탐방: 방문할 수 있는 노드는 모두 방문하고 종료함

* 알파고는 딥러닝, `탐색 기법` 통해 수를 읽음. (몬테카를로 트리 탐색)

## 상태(state), 상태공간(state space), 연산자
* 탐색(search)는 상태공간에서 시작상태에서 목표상태까지의 경로를 찾는 것
* 상태공간 (state space): 상태들이 모여 있는 공간 
* 연산자: 다음 상태를 생성하는 것 
* 초기상태: 처음에 주어진 상태
* 목표상태: 말그대로 목표상태임 
* ex> 8-puzzle에서의 연산자 
    * 빈칸이 움직인다고 생각 -> 4개의 연산자 (아니면 32개의 연산자가 생겨버림..)

## 경로 찾기 문제 예시 
* 트리가 `아님`! (왜? : 경로들이 유일하지 않음. A에서 C로 가는 경로가 유일하지 않음 등..)
* 상태? 
    * 초기 상태: A
    * 목표 상태: F
    * List로 표현 가능 : 초기상태 (A) 에서 목표상태 (A...F)
* 연산자? 
    * 가능한 경로 중 하나의 경로를 선택하는 것 

## N-queen 문제 
* 8-queen 문제는 8x8 체스판에 두 개의 퀸이 서로를 위협하지 않도록 8개의 퀸을 배치하는 문제 
* 상태? 
* 연산자? 
    * place_i 연산자는 새로운 퀸을 i번째 행에 배치한다. 
    * 연산자 집합 O = { place_i | 1<=i<=8 }

## 탐색 트리 
* 상태 = 노드 (node)
* 초기 상태 = 루트 노드
* 연산자 = 간선(edge)
    * 연산자 적용하기 전까지는 탐색 트리는 미리 만들어져 있지 않음. 
* ex> 출발 도시에서 목표 도시까지의 경로 탐색 
    * 확장이 끝난 노드 (닫혀있는 노드, 방문한 노드)
    * 열려 있는 노드 (생성되었지만 아직 확장되지는 않은 노드, 아직 방문x 노드): 선택할 수 있는 노드들. 

## 기본적인 탐색 기법 
* ppt 19 그림을 참고 
* 맹목적인 탐색(Blind search method): 목표 노드에 대한 정보를 이용하지 않고 기계적인 순서로 노드를 확장하는 방법. 매우 소모적인 탐색.
    * 깊이 우선 탐색 (DFS)
    * 너비 우선 탐색 (BFS)
    * 균일 비용 탐색 
* 경험적인 탐색(heuristic search method): 목표 노드에 대한 경험적인(heuristic) 정보를 사용하는 방법. -> 효율적인 탐색이 가능! 
    * 탐욕적인 탐색 (greedy)
    * A* 탐색 

## 탐색 성능 측정
* 완결성(completeness): 문제에 해답이 있다면, 반드시 해답을 찾을 수 있는지
* 최적성(optimality): 가장 비용이 낮은 해답을 찾을 수 있는지 여부 
* 시간 복잡도(time complexity): 해답을 찾는데 걸리는 시간
* 공간 복잡도(space complexity): 탐색을 수행하는 데 필요한 메모리의 양 
    * b: 탐색 트리의 최대 분기 계수
    * d: 목표 노드의 깊이 depth 
    * m: 트리의 최대 깊이 