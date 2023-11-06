# CHAP2 분할 정복법 

## 분할 정복법 (Divide and Conquer)
* 문제의 사례를 2개 이상의 더 작은 사례로 나누어(divide) 각 작은 사례에 대한 해답(conquer)을 쉽게 얻을 수 있으면 -> 이들의 해답을 결합(combine)하여 원문제의 해답을 얻는 방식
    * 문제를 나누는 과정은 해답 얻을때까지 반복적으로 적용
    * 작은 사례는 원문제와 같으나 작은 규모
    * 하향식(top-down) 접근 방법
        1. `분할(Divide)`: 해결하기 쉽도록 문제를 여러 개의 작은 부분으로 나눔
        2. `정복(Conquer)`: 나눈 작은 문제를 각각 해결
        3. `통합(Combine)`: (필요시) 해결된 해답 모음 
    * 보통 재귀 프로세서로 먼저 해결 (보다 효율적인 반복 프로시저가 없는지 검토함)

## 이진 검색 (Binary Search)
* 정렬된 리스트(배열)에 대한 검색 
* 과정
    1. 찾고자하는 x가 중간 항에 있으면 종료, 아니면
    2. 분할: 배열을 중간으로 이등분하여 2개의 부분 배열 -> x가 중간항에 있는것보다 작으면 왼쪽 부분배열, 크면 오른쪽 부분배열 선택
    3. 정복: 선택한 부분 배열에서 x 찾음. 부분배열의 크기가 너무 적지 않은 이상 재귀방법으로 찾음 
    4. 통합: 원 해답을 부분 배열에 대한 해답으로부터 얻음 
    ```java
    int BinSearch(index low, index high){
        if(low>high) return -1;
        else{
            index mid = ⌊(low + high)/2⌋;
            if(x==S[mid]) return mid;
            else if(x<S[mid]) BinSearch(low,mid-1);
            else BinSearch(mid+1,high);
        }
    }
    ```
* 이진 검색은 n,x,S가 파라미터로 사용되지 않음 
    * 이유: 재귀호출되면서 변경되지 않으므로 재귀호출할떄마다 가지고 다니는 것은 극심한 낭비이기 때문에 뺌 
* best case: 바로 mid에 x가 있을 때
* worst case: x가 배열에 있는 모든 원소보다 클 때 

## 합병 정렬 (Merge Sort)
* 쌍방합병(two-way merging): 같은 순으로 정렬되어 있는 두 개의 배열을 정렬된 하나의 배열로 만드는 과정 -> 합병을 반복적으로 적용하여 배열 정렬 가능 
* 과정
    1. 분할: 배열을 n/2개의 요소로 구성된 2개의 부분 배열로 분할 
    2. 정복: 각 부분 배열을 정렬
    3. 통합: 정렬된 각 부분 배열을 하나의 정렬된 배열로 합병 
    ```java
    void mergeSort(int n, keytype S[]){
        if(n>1){
            int h=⌊n/2⌋, m=n-h;
            keytype U[1..h], V[1..m];
            copy S[1..h] to U[1..h];
            copy S[h+1..n] to V[1..m];
            mergeSort(h,U);
            mergeSort(m,V);
            merge(h,m,U,V,S); 
        }
    }

    void merge(int h, int m, keytype U[],keytype V[], keytype S[]){
        index i=1,j=1,k=1;
        while (i<=h && j<=m){
            if(U[i]<V[j]){
                S[k]=U[i];
                i++;
            }
            else{
                S[k]=V[j];
                j++;
            }
            k++;
        }
        if(i>h) copy V[j..m] to S[k..h+m];
        else copy U[i..h] to S[k..h+m]; 
    }
    ```
## 빠른 정렬

## 최소-최대 찾기 