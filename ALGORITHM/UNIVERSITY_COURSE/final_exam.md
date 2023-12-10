# 기말고사 대비 의사코드 정리 
chap 4(Greedy algorithm)~chap 7(sorting algorithm)

## CHAP4 
### 거스름돈 문제 알고리즘 
```c
void minChange(int changes[],int amount,int &cc){
    cc[]={0};
    for(i=1;i<=r;i++){
        while(amount>=changes[i]){
            cc[i]++;
            amount=amount-changes[i]; 
        }
    }
} 
```
### 최소 비용 신장 트리(MST, Minimum Spanning Tree)
최소비용 신장트리를 구하는 브루트포스 알고리즘의 시간복잡도: 지수 시간보다 나쁨 
#### Prim's 알고리즘 
```c
void prim(int n, number W[][], EdgeSet& F){
    index i,vnear;
    number min; 
    edge e;
    index nearest[2..n]; number distance[2..n]; 
    F=∅;
    for(i=2;i<=n;i++){
        nearest[i]=1;
        distance[i]=W[1][i];
    }
    for(j=1;j<=n-1;j++){ //신장 트리는 간선 개수가 항상 n-1개여야하므로 => (n-1)반복 
        min= ∞;
        for(i=2;i<=n;i++){ //Y와 가장 가까운 정점을 검색 //(n-1)번 반복 
            if(0<=distance[i]<min){
                min=distance[i];
                vnear=i; 
            }
        }
        e=(nearest[vnear],vnear); 
        F=F ∪{e};
        distance[vnear]=-1; //방문했으므로 -1 
        for(i=2;i<=n;i++){ //추가된 정점을 포함하여 nearest와 distance 갱신 //(n-1)번 반복 
            if(W[i][vnear]<distance[i]){
                distance[i]=W[i][vnear]; 
                nearest[i]=vnear; 
            }
        }
    }
}
```
    * 기본연산: 비교연산
    * 입력크기: n
    * T(n)=((n-1)+(n-1))*(n-1)= 2(n-1)(n-1)  ∈ Θ(n^2)

#### 분리(부분) 집합 (disjoint set) 자료구조 
```c
void initial(int n){
    for(i=0;i<=n;i++)
        parent[i]=-1; 
}

set_pointer find(index i){ //i노드가 속한 트리의 루트 노드 리턴 
    for(;parent[i]>=0;i=parent[i])
        ;
    return i; 
}

bool equal(set_pointer p,set_pointer q){//같은 트리인지 여부 (루트가 같으면 같은 트리)
    if (p==q) return true;
    else      return false; 
}

void merge(set_pointer p, set_pointer q){//두 트리를 합침 
    parent[q]=p; 
}
```

#### kruskal 알고리즘
```c
void kruskal(int n,int m,EdgeSet E, EdgeSet& F){
    index i,j;
    Set_pointer p,q;
    edge e;
    VertextSet V[1..n];
    E에 속한 m개의 간선을 가중치에 따라 오름차순으로 정렬
    F = ∅;
    initial(n); 
    while(|F|<n-1){
        e=아직 고려하지 않은 가중치가 최소인 간선; 
        i,j=e에 의해 연결되는 정점의 색인
        p=find(i); //Vi가 포함되어 있는 집합
        q=find(j); //Vj가 포함되어 있는 집합 
        if(!equal(p,q)){
            merge(p,q);
            F=F U {e}; 
        }
    }
}
```
    * 최악의 경우
        * 기본연산: 비교
        * 입력크기: 정점 수 n, 간선 수 m
        * 정렬에 소요되는 비용: Θ(mlgm)
        * while 루프: 최악의 경우 모든 간선 고려 (m번 반복) 
            * 루프 전체 비용: Θ(mlgm)
        * V[i]집합 초기화 비용: Θ(n)
        * n<m이므로 Θ(mlgm) 
        * n-1<=m<=n(n-1)/2 -> 최악의 경우 n(n-1)/2이므로 Θ(n^2lgn)

### Dijkstra 단일출발점 최단경로 문제 
시간복잡도: Θ(n^2)
Prim의 MST 알고리즘과 유사함 
```C
void dijkstra(int n,const number W[][], EdgeSet& F){
    index i, vnear, touch[2..n];
    edge e;
    number length[2..n];
    int min; 
    F = ∅;
    for(i=2;i<=n;i++){ //초기화 => n-1 
        touch[i]=1;
        length[i]=W[1][i];
    }
    for(j=1;j<=n-1;j++){//n-1번 
        min = ∞;
        for(i=2;i<=n;i++){ //n-1 
            if(0<=length[i]<min){
                min=length[i];
                vnear=i; 
            }
        }
        e=(touch[vnear],vnear);
        F = F∪{e};
        for(i=2;i<=n;i++){ //n-1 
            if(length[vnear]+W[vnear][i]<length[i]){
                length[i]=length[vnear]+W[vnear][i];
                touch[i]=vnear; 
            }
        }
        length[vnear]=-1; 
    }
}
```
    * T(n)=2(n-1)(n-1) ∈ Θ(n^2)

### 배낭 채우기 문제 
6, 7장에서 다룸
* 브루트포스 알고리즘 시간 복잡도: 2^n

### 배낭 빈틈없이 채우기 문제
```c
void GreedyKnapsack(float M, int n, int p[], int w[], float& x[]){
    for(int i=1;i<=n;i++) x[i]=0.0; //초기화
    float U=M; 
    for(i=1;i<=n;i++){
        if(w[i]>U) break; 
        x[i]=1.0;
        U-=w[i]; 
    }
    if (i<=n) x[i]=U/w[i]; //item이 남으면 나눠서 들어감 
}
```
    * p[1..n], w[1..n] 의 배열 정렬: Θ(n log n)
    * GreedyKnapsack: Θ(n)
    * T(n)= Θ(n log n)

### 최적 머지 패턴 알고리즘
```c
struct treenode{
    struct treenode *lchild, *rchild; 
    int #ofrecords;
};

typedef struct treenode Type; 

Type *Tree(int n){
    for(int i=1;i<=n-1;i++){
        Type *pt=new Type;
        pt->lchild=Least(list);
        pt->rchild=Least(list);
        pt->#ofrecords=(pt->lchild)->#ofrecords+(pt->rchild)->#ofrecords; 
        insert(list,*pt);
    }
    return (Least(list)); 
}
```
    * 시간복잡도: Θ(n^2) or Θ(n log n)

### 최적 이진 코드(허프만 코드)
```c
struct nodetype{
    char symbol; //문자
    int frequency; 
    nodetype* left;
    nodetype* right; 
};

Nodetype Huffman(struct nodetype charSet[],int n){
    for(i=1;i<=n;i++)
        Insert(PQ,charSet[i]); //우선순위 큐 사용 
    for(i=1;i<=n-1;i++){
        p=remove(PQ);
        q=remove(PQ);
        r=new nodetype;
        r->left=p; 
        r->right=q; 
        r->frequency=p->frequency+q->frequency;
        insert(PQ,r); 
    }
    return remove(PQ); 
}
```
    * 시간복잡도: 최적 머지 패턴과 동일 Θ(n^2) or Θ(n log n)
    * 우선순위큐(PQ)