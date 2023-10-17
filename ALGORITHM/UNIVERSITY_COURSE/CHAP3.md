# CHAP 3

## 동적 프로그래밍

## 이항계수 계산 알고리즘

## 이진 검색 트리 개수 구하기

## 최적 이진 검색 트리

## Floyd의 최단 경로 알고리즘
* 최단 경로 찾기 문제
    * **무작정(???) 알고리즘(brute-force algorithm)**: 한 정점에서 다른 정점으로의 모든 경로의 길이를 구한 뒤 그들 중 최소 길이를 찾는 방법
        * 경로 개수: **(n-2)!** = (n-2)(n-3)..1 -> `비효율적`
    * 최단 경로 찾기 문제는 **`최적화 문제(optimization problem)`** 중 하나 
        * 최적화 문제: 답이 여러개 존재 가능 
        * 후보 답 중 가장 최적의 값(최적값/ 보통 최대 or 최소값)을 가지는 답을 하나 찾는 것
        * 방법
            1. 모든 경로 찾은 후 그 중에서 최단 경로 찾기 : (n-2)!개 존재 -> 비효율적
            2. **동적 프로그래밍**: n^3 알고리즘 가능 
* Floyd의 최단 경로 알고리즘 
    ```java
    void floyd(int n, const number W[][], number D[][]){
        int i,j,k;
        D=W;
        for(k=1;k<=n;k++)
            for (i=1;i<=n;i++)
                for(j=1;j<=n;j++)
                    D[i][j]=minimum(D[i][j],D[i][k]+D[k][j]); 
    }
    ```
    * 2차원 배열 P 사용 (중간 노드 정보 저장)
        ```java
        void floyd2(int n, number W[][],number D[][],number P[][]){
            index i,j,k;
            for (i=1;i<=n;i++)
                for (j=1;j<=n;j++) P[i][j]=0; 
            D=W;
            for(k=1;k<=n;k++)
                for(i=1;i<=n;i++)
                    for(j=1;j<=n;j++)
                        if (D[i][k]+D[k][j]<D[i][j]){
                            P[i][j]=k;
                            D[i][j]=D[i][k]+D[k][j]; 
                        }
        }
        ```
    * 최단 경로 출력하기 
        ```java
        void path(index q,r){
            if (P[q][r]!=0){
                path(q,P[q][r]);
                print " v" P[q][r];
                path(P[q][r],r); 
            }
        }
        ```
        
## 최적의 원칙 

