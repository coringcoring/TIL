# CHAP 05

* **`병행(Concurrent)`**: 같이 존재하고 있다
    * ex> **다중 프로그래밍**에서 메모리에 다수의 프로세스가 같이 존재하는 것 
    * 단일처리기의 경우: `논리적 병행성(Logical Concurrency)`
    * 다중처리기의 경우: `물리적 병행성(Physical Concurrency)`
    * 병행 프로세스들은 서로 간에 **`비동기적(Asynchronous)`** => **공유**하는 자원이나 데이터가 있는 병행 프로세스들이 각자 **비동기적**으로 실행되는 것을 제대로 관리하지 못한 채 방치할 경우 문제가 발생함 
* **`병렬(Parallel)`**(!=`병행`): **다중처리 시스템**에서 여러 개의 프로세스가 병렬로 실행됨. 
* => 프로세스들의 **병행성**은 **처리기의 수**와 관계 없으나, **병렬 처리**가 성공하기 위해서는 **병행성**이 전제되어야함 
    * 프로세스가 실행되기 위해서는 **메모리**에 올라와있어야하므로.. 

## 병행 프로세스 (Concurrent Process)
* `공유된` **자원**이나 **데이터**가 있을 경우, 이 자원에 대해 **병행 프로세스**들이 따르는 **룰**이 필요함
    * 룰: *한 번에 한 프로세스만이 접근하도록 하고, 해당 자원에 대해 의도했던 실행을 완료하도록 보장한다* 
    * 책에 있는 예시 참고. count라는 **공유 변수**에 대하여 **시간 종료**나 **우선순위**같은 이유에 의해 CPU를 뺏기더라도 **조작 도중**이었던 공유 변수 count에 대한 접근을 막아야함. 

## 상호 배제 (Mutual Exclusion)
* **`경쟁 상태 (Race condition)`**: 프로세스들이 **공유 데이터**에 대해 서로 접근을 시도하는 상황 
    * 이로 인해 `상호 배제`, `교착 상태(Deadlock)`, `기아(Starvation)` 문제 발생 
* 임계 자원(Critical Resource), 임계 영역 (Critical Section)
    * **`임계 자원(Critical Resource)`**: 두 개 이상의 프로세스가 동시에 사용할 수 없는 자원
    * **`임계 영역(Critical Section)`**: 임계자원에 접근하고 실행하는 **프로그램** 내의 **코드** 부분 
* **`상호배제`**: 한 번에 하나의 프로세스만이 **임계영역**에 들어가야(실행되야) 하는 것 
* 임계영역의 성공적인 실행을 위해 **상호 배제**는 지켜져야함
* 임계영역에 있지 않는 프로세스가 다른 프로세스의 임계영역 진입을 막아서도 **x** 
* **비어있는** (아무도 실행하고 있지 않은) **임계영역**에 대한 진입은 바로 **허용**하되, 특정 프로세스의 진입 시도가 계속 **무산**되어 **기아**를 겪지 않도록 해야함 
* 임계영역에 들어가고자 하는 프로세스는 임계영역 내에 다른 프로세스가 있다면 기다려줘야하고, 임계영역이 비어있다면 진입하여 다른 프로세스가 들어오지 못하도록 해야함 
* 임계영역을 벗어날때는 자신이 나오는 사실을 알려 다른 프로세스가 들어올 수 있도록 해야함 
* **상호배제의 성공** 여부를 결정짓는 것: **임계영역**을 진입할때와 나올때 꼭 해야하는 일을 잘 구현하여 프로그램 내 **임계영역** 앞뒤에 적절하게 코딩해주느냐 가 결정 

## 상호배제를 위한 소프트웨어 기법들 
* **`소프트웨어 기법`**들: **병행**하는 프로세스들에게 **상호배제**를 책임지도록 한 것 
    * **운영체제**의 지원 없이 프로세스들 간에 자신의 **프로그램**에서 **임계영역 앞뒤**로 적절한 **코딩**을 통해 **상호배제**를 해결하는 방식 
        * 단점: **CPU 낭비** 가능, **프로그래머의 실수**로 인한 **오류** 발생 가능 
* **`parbegin/parend`** 구조 
    ```
    parbegin
        statement_1;
        statement_2; 
        ...
        statement_n;
    parend
    ```
    *` parbegin`과 `parend` 사이에 존재하는 **n개의 문장**들이 **동시에** 수행될 수 있음 
    * **단일처리기** 시스템의 경우: 문장의 수행 순서를 **임의로 진행**해도 좋다는 의미 
    * **다중처리기** 시스템의 경우: 각 문장을 **병렬**로 실행하겠다는 의미 
        => 문장들의 **나열 순서**는 아무 **의미x** 
    * *병행 프로세스 간의 **실행 속도**와 **임계영역**을 들어가고자 하는 **횟수**의 **차이**는 얼마든지 날 수 있음*
### 몇가지 미완성 시도들 
1. 첫번째 시도 
    ```
    Begin /*main*/
        int turn=0;
        parbegin
            P0;
            P1;
        parend
    End
    ```
    ```
    P0:
    While (true) do
        .
        .
        While (turn ==1); /* do nothing*/
        <critical section>;
        turn :=1; 
        .
        .
    endwhile;
    ```
    ```
    P1:
    While (true) do
        .
        .
        While (turn ==0); /*do nothing*/
        <critical section>;
        turn :=0;
        .
        .
    endwhile; 
    ```
    * 허점 
        1. turn의 초기값이 0으로 되어있으므로 임계영역의 **첫번째 진입**은 **P0**만 할 수 있음 -> **임계영역**이 **비어있을** 경우 **진입**을 원하는 프로세스를 **방해**해서는 안된다는 원칙 **위배**
        2. 임계영역의 진입이 **turn 값**을 바꾸어줌으로써 가능하므로 P0와 P1은 한번씩 **번갈아가며** 진입이 가능, 누구나 **연속해서 두 번 이상** 진입 **불가능** 
            * 프로세스 각자의 진입을 원하는 횟수는 **다를 수 있음**
            * 상대적으로 **많은 횟수**의 진입이 요구되는 프로세스는 그 횟수 만큼 상대 프로세스가 **임계영역**을 진입해주어야함 (만약 상대 프로세스가 먼저 종료될 경우 자기도 임계영역에 들어갈 수 없는 문제 발생) 
            * 상대적으로 **실행 속도가 느린** 프로세스의 속도에 **의존적**일 수 밖에 없음 
2. 두번째 시도 
    ```
    boolean flag[0], flag[1]; 
    void P0(){
        while(true) {
            .
            .
            while(flag[1]); 
            flag[0]=true;
            <critical section>;
            flag[0]=false; 
            .
            .
        }
    }

    void P1(){
        while(true){
            .
            .
            while(flag[0]); 
            flag[1]=true;
            <critical section>;
            flag[1]=false; 
            .
            .
        }
    }

    void main(){
        flag[0]=false;
        flag[1]=false;
        parbegin
            P0;
            P1;
        parend 
    }
    ```
    * 나아진점: 첫번째와 달리 **임계영역 최초 진입의 제한**이 사라짐, 상대적으로 **많은 횟수**의 진입이나 상대 프로세스가 **먼저 종료**되어도 **임계영역**에 진입이 가능 
    * 허점
        1. 각자의 **flag를 true로 만드는 작업**이 **while문 다음**에 있어서 발생하는 문제
            * 만약 P0에서 flag[1] 검사 후 while문 벗어난 다음 flag[0]을 true로 만들기 전에 CPU를 P1에게 뺏기게되면 P1은 while문을 지나 임계영역에 들어가게되고 실행 도중 다시 P0에게 CPU가 넘어가게 되면 P0는 이전에 중단되었던 작업(flag[0]을 true로 만드는 작업) 진행 후 임계영역에 진입 -> *둘 다 임계영역에 존재하게 되어버림* 
            * **다중처리**의 경우: 동시에 while문 검사한 후 둘 다 각자 flag를 true로 만든 다음 임계영역으로 진입할 수 있기 때문에 *상호배제가 지켜지지 않게됨* 
3. 세번째 시도
    * 위의 코드에서 flag를 true로 만드는 작업이 **while문 앞**에 오는것 
    * 발생하는 문제: P0가 flag[0]을 true로 만든 다음 CPU가 P1에게, P1은 flag[1]을 true로 만든 다음 while문에서 맴돌다가 다시 CPU는 P0에게, P0역시 while문에서 맴돌게 되는 현상 발생 가능 => *둘 다 임계영역에 진입하지 못하게됨* **`데드락(DeadLock)`**
4. 네번째 시도 : 두 프로세스의 **속도**가 교묘히 맞물렸을 때 둘 다 **임계영역**에 진입하지 못하게되는 현상 **`라이브락(Livelock)`**
    * 그러나 속도의 차이가 조금이라도 어긋나는 시점이 오면 하나가 임계영역에 들어갈 수 있음 
### 성공적인 기법들 
* **`Dekker의 알고리즘`**
    * 단점: 이해하기 어렵고 정확성을 증명하기 까다로움
* **`Peterson의 알고리즘`**
    * Dekker 알고리즘의 단점을 해결 
        ```
        void P0(){
            While (true){
                flag[0]=true;
                turn =1; 
                While (flag[1]&& turn==1); /*do nothing*/
                <critical section>;
                flag[0]=false;
                <remainder>; //임계영역이 아닌 해당 프로세스 고유의 일 
            }
        }
        ``` 
### n 프로세스 간의 상호배제를 위한 소프트웨어 기법들 
* **`n개의 프로세스들을 대상으로 하는 상호배제 기법`**들..
1. **`Dijkstra의 알고리즘`** : 그러나 `무한 대기`가 발생할 수 있음
2. **`Knuth의 알고리즘`**: `지연 시간`이 커지는 단점이 있음 
3. **`Eisenberg와 Mcquire의 알고리즘`**: `유한 시간` 내에 `임계영역`으로의 진입이 보장됨 (`성공`)
4. **`Bakery 알고리즘 (=Lamport의 알고리즘)`**
    * 원래 분산 시스템용으로 소개되었음
    * n개의 프로세스들을 대상으로 하는 상호배제 기법으로도 유용함 
    ```
    do{
        choosing[i]=true;
        number[i]=max(number[0],number[1],...,number[n-1])+1;
        choosing[1]=false;
        for(j=0;j<n;j++){
            while(choosing[i]); 
            while((number[i]!=0) && ((number[j],j)<(number[i],i))); 
        }
        <critical section>;
        number[i]=0;
        <remainder>; 
    }while(1);
    ```
    * 장점: `number[i]=0`으로 초기화해줌으로써 다른 프로세스에게 **임계영역의 진입 기회**를 주게됨-> `번호값`에 의해 `차례`가 정해지므로 특정 프로세스의 **무한 대기는 발생 x** (`기아 x`)

* 소프트웨어 기법들의 단점
    * **운영체제**의 특별한 지원 없이, **프로세스 간 협력**을 통해 상호배제를 실현하는 것이므로 실행 시의 **부하**가 큼. 
    * **실수**로 인한 **오류의 가능성**도 높음 
    * **`바쁜 대기(Busy Wait)`** 또는 **`스핀락(SpinLock)`** 발생: 임계영역의 `중복 진입`을 막기 위해 `while문`을 계쏙 맴도는 것은 CPU는 가동은 하였으나 유용한 곳에 쓰지 못한 채 낭비하는 결과를 초래함 
* 임계영역에 대한 실행은 *"한 번에 한 프로세스만 하겠다"* 라는 의미 => *"차례대로"* => **`동기화(Synchronization)`**

## 상호배제를 위한 하드웨어 기법들 
### 인터럽트 금지를 사용한 기법
* `단일처리 시스템`에서: CPU를 뺏길 수 있는 `인터럽트`를 임계영역의 실행을 완료할 떄까지 발생하지 않도록 하면 됨 
    ```
    While (true) do
        .
        .
        Interrupt Disable; 
        <critical section>;
        Interrupt Enable; 
        .
        .
    endwhile; 
    ``` 
    * 문제
        1. 인터럽트를 금지시킴으로써 **시스템의 효율적인 운영 방해** 쉬워짐 (인터럽트가 발생하는 이유는 *긴급한 문제*가 발생시이므로)
        2. 인터럽트 금지는 `처리기` 단위-> `다중처리 시스템`에서 다른 처리기에서 실행되는 프로세스로부터의 `접근 가능성`은 여전히 존재하기 때문에 사용하기 힘든 방법임 
* 다른 기법: **메모리**의 **같은 위치**에 대한 **읽기,쓰기** 또는 **읽기, 검사**와 같은 일을 `한 명령어 사이클`동안 (`한번의 접근에`) 처리해주는 `(기계)명령어`
    * -> 다른 접근 요청이 차단된 가운데, **`원자적(Atomic)`**으로, **`실행 동안 끊기지 않고 (Indivisible)`** 완료될 수 있다. 
### 하드웨어 명령어를 사용한 기법 
1. **`testandset`** 기계명령어
    ```
    boolean testandset(boolean &target){
        boolean rv :=target;
        target := true;
        return rv; 
    }
    ```
    * testandset을 이용한 상호배제
        ```
        const int n = ...; //프로세스 개수
        boolean lock;
        void P(int i){
            while(true){
                while(testandset(lock)); 
                <critical section>;
                lock :=false; 
                <remainder>; 
            }
        }
        void main(){
            lock:=false; 
            parbegin
                P(1),P(2),...P(n);
            parend; 
        }
        ```
2. **`exchange(=swap)`** 기계명령어
    ```
    void exchange(boolean &r, boolean &m){
        boolean temp = r;
        r = m; 
        m = temp; 
    }
    ```
    * exchange를 이용한 상호배제
        ```
        const int n=...; //프로세스 개수
        boolean lock;
        void P(int i){
            while(true){
                key=true; //local variable
                while (key==true) do exchange(key,lock); 
                <critical section>;
                lock :=false; 
                <remainder>; 
            }
        }
        void main(){
            lock:=false;
            parbegn
                P(1),P(2),...P(n); 
            parend; 
        }
        ``` 
* 기계명령어 사용의 장점
    * 간단하다
    * `다중처리 시스템`에서도 쉽게 쓸 수 있다
    * `한 프로그램` 내에서 `서로 다른 변수`를 사용하여 **여러 개의 임계영역**도 지원할 수 있음 (ex> lock1, lock2...)
* 기계명령어 사용의 단점
    * 여전히 `바쁜 대기`를 함 -> CPU 낭비
    * `차례`가 정해지지 않으므로 어떤 프로세스는 `기아`를 겪을수도 있음
    * 임계영역 실행 중 `높은 우선순위`를 가지는 프로세스에게 CPU를 뺏길 경우 `교착 상태`에 빠질 우려 존재 

## 세마포어 (Semaphore)
* **`세마포어`**
    * Dijkstra가 1965년에 제안
    * 3 개의 특수한 명령들만 접근할 수 있게 허용된 **`보호된 변수`**
    * 앞서 다루었던 것들보다 더 **`높은 수준`**에서 **상호배제 명령**을 구현할 수 있게 함
        * `더 높은 수준`: **프로그래밍 언어+ 운영체제 수준** 에서 **병행성**을 위해 제공되는 기법
* 세마포어의 종류 : 그 `변수`가 가질 수 있는 **값의 범위**에 따라 종류가 구분됨 
    1. **`이진(Binary) 세마포어`** : 어떤 세마포어가 0 또는 1의 **이진값**만을 가진다면 
    2. **`계수(Counting)`** 혹은 **`정수(Integer)`** 세마포어 : 세마포어의 값이 **음이 아닌 모든 정수**가 될 수 있다면 
* 세마포어를 위한 특수한 명령들 : **`비분리(Indivisible) 명령`**들임 
    1. 세마포어 값을 `초기화`하는 명령 
    2. **`P명령`** : 세마포어인 S만을 `매개변수`로 함 (`S`에 대한 접근은 `P`와 `V `명령을 통해서만 허용되어짐)
    3. **`V명령`** : 세마포어인 S만을 `매개변수`로 함 (`S`에 대한 접근은 `P`와 `V` 명령을 통해서만 허용되어짐)
        * 세마포어에 대한 명령들은 각각 **분리되지 않고** 수행될 수 있도록 구현해야함
        * **같은 세마포어**에 대해서 **동시에** 실행되지 못함! 
        ```
        //block & wakeup 방식 
        P(S): if (S>0) then S = S - 1; 
                       else S>0 조건이 만족될 때까지 큐에서 대기; 
        V(S): if (큐에서 대기 중인 프로세스들이 존재)
                        then 그 중의 한 프로세스를 준비 또는 실행 상태로 만듦; 
                        else S = S + 1; 
        ``` 
        * `큐`와 관련된 부분은 `운영체제`가 해 줄 일이므로 이 내용을 빼고 표현한다면..
            ```
            //busy-wait 방식
            P(S){
                while(S<=0); //busy wait
                S--;
            }
            V(S){
                S++;
            }
            ```
* 세마포어를 이용한 상호배제 알고리즘 : Binary Semaphore 사용 
    ```
    const int n= .. /*프로세스의 개수*/
    semaphore s=1; //binary semaphore: 0 아니면 1의 값 
    void p(int i){
        while (true){
            P(S);
            <critical section>;
            V(S);
            <remainder>; 
        }
    }
    void main(){
        parbegin
            P(1),..P(n);
        parend; 
    }
    ```
* **`정수 세마포어(Integer Semaphore)`**의 사용 
    * 프린트는 `공유 자원`으로서 `Pool`로 관리되고 있음
    * 최대 10명이 동시 작업을 할 수 있다고 가정
    * 임계 영역은 출력 작업 
    * 임계영역의 진입을 세마포어를 사용해 10명까지 허용하는 알고리즘으로 관리 가능 
        * S를 10으로 초기화
        * `<critical section>` 부분을 출력 작업으로 
        * **S값이 10보다 작은 양수 값** = **작업이 가능한 프린터 대수** (*임계영역 진입이 허용되는 프로세스 수*)
    * **공유 자원의 풀 관리에 유용한 정수 세마포어** 
* 세마포어의 장점 
    1. **`높은 수준`**에서 **상호배제 명령**을 구현할 수 있게 함 
    2. 프로세스 간의 진행이 **`상호의존적인 관계`**라서 **`동기화`**가 요구될 때 세마포어가 유용함 
        * `동기화`: *"서로를 의식하고 실행의 보조를 맞추다"*
        * ex> P0는 실행 도중 P1이 건네주는 데이터를 받아야 계속 실행이 가능한 경우 
            * 이진 세마포어 sync =0 (초기값)
            * P0의 데이터 수신 확인 P(sync)
            * P1의 데이터를 만들어 보냈을 때 V(sync)
    3. **`Resource pool`** 관리에 유용 

## 생산자-소비자 문제(Producer-consumer Problem)
* 병행 프로세스의 상호배제와 동기화를 다룰 때 발생하는 고전적인 문제들
    * 생산자-소비자 문제
    * 읽기와 쓰기 문제
    * 식사하는 철학자 문제
    * 흡연가 문제
    * 이발소 문제 
* **`생산자-소비자 문제`**
    * `생산자`: `데이터`를 만들어 `버퍼`에 채우는(`저장`) 프로세스
    * `소비자`: `버퍼`에 있는 `데이터`를 꺼내 `소비`(비움)하는 프로세스 
    * `상호배제`: `버퍼`는 `공유자원` -> 버퍼에 대한 접근 (저장,꺼내는 일)이 `상호배제` 되어야함
    * `동기화`: 버퍼가 비어있을 때는 소비자가, 버퍼가 꽉 차있을떄는 생산자가 기다려야함 
* **`원형 유한 버퍼`**에 대한 생산자-소비자 관계 
    ``` 
    생산자:
    while (true){
        create data V;
        while ((in+1)%n ==out);
        buffer[in]=V; //append data 
        in = (in+1)%n;
    }
    ```
    ```
    소비자:
    while (true){
        while (in==out);
        W=buffer[out]; //take data
        out=(out+1)%n;
        consume data W; 
    }
    ```
* **`원형 유한 버퍼`**에서 `세마포어`를 사용한 생산자-소비자 알고리즘
    ```
    semaphore s=1;
    semaphore f=0;//full 
    semaphore e=n; //n=buffer size, empty 
    void producer(){
        while(true){
            produce data V;
            P(e); //empty가 0이면 full ->기다려야 
            P(s); //버퍼에 대한 접근 상호배제 
            append data V; 
            V(s);
            V(f); // full +1 증가 
        }
    }
    void consumer(){
        while(true){
            P(f); //full이 0이라면 empty -> 기다려야함 
            P(s);
            take data W;
            V(s);
            V(e); //empty +1 증가 
        }
    }
    void main(){
        parbegin
            producer(),consumer();
        parend
    }
    ```
* (공간의 개수를 정의하지 않은) `무한 버퍼`를 가정한 알고리즘 
    * e에 대한 변수와 명령어들 제거하면 됨 
* 주의할 점
    * 여러 개의 세마포어 사용시 -> `위치`를 세심하게 따져야함 
        * ex> `무한 버퍼용 알고리즘`의 경우 `소비자 프로그램` 부분에서 `P(f)`와 `P(s)`의 순서를 바꾼다면..
            * P(s)를 통과해 버퍼에 대한 배타적 접근 권리를 가진 소비자가 버퍼가 비어있음을 발견하게 될 경우 P(f)에서 대기 상태 
            * 버퍼를 채워줄 생산자는 P(s)에서 대기하게 되어 `교착 상태` 만들게 됨 
            * 알고리즘은 실패! 
* 세마포어를 사용하는 기법: **`block and wakeup`** 기법
    * `block and wakeup` 기법: `운영체제 수준`에서 `임계영역`으로의 진입을 기다리는 프로세스들을 `대기 상태`로 전환시킴 -> 바쁜 대기(busy-wait)를 할 때의 `CPU 낭비`를 없앨 수 있음 
    * 단점
        * 프로세스를 `대기상태`로 만드는 일에 드는 `비용`
        * `임계영역`이 매우 짧게 끝나는 경우 바쁜 대기에 비해 프로세스 반응이 **즉각적이지 못함** 
        * `대기 중`인 프로세스들에 대한 다음 차례의 `임계영역 진입`을 위한 `선택 기준`이 존재하지 않음 (존재하는거: Bakery 알고리즘) -> 누가 얼마나 기다렸는지를 고려하지 않음 -> 특정 프로세스들의 **기아 유발 가능** 

## Eventcount와 Sequencer를 사용한 기법 
* `Eventcount`와 `Sequencer` -> 특별한 명령들에 의해서만 접근이 가능한 `정수형 변수`들 
    * 초기 값= 0 -> 값이 **감소하지 x** 
* **`ticket(s)`** 명령: `비분리`로 실행됨. `sequencer 변수`인 s를 반환해줌. (실행될때마다 s+1)
* **`Eventcount 변수`**인 E에 대한 명령 
    * `read(E)`: 현재 E값을 반환해줌
    * `advance(E)`: 1 증가시킴
    * `await(E,v)`: E가 v보다 작으면 기다리도록 함 
    * => `비분리`로 실행되지 않아도 좋음 
* Eventcount와 Sequencer를 사용한 생산자-소비자 알고리즘
    ```
    생산자 i:
    var pord: integer; //producer order 생산자 순서
    while (true){
        create data V;
        pord=ticket(p);
        await(in,pord);
        await(out,pord-n+1); 
        buffer[pord%n]=V;
        advance(in); 
    }
    ```
    ```
    소비자 i:
    var cord: Integer; 
    while(true){
        cord=ticket(s);
        awiat(out,cord);
        await(in,cord+1);
        W=buffer[cord%n];
        advance[out];
        consume data W; 
    }
    ```
    * 생산자와 소비자가 여러 명 있는 환경 수용 위해 
        * sequencer 변수: p,c
        * eventcount 변수: in, out
        * 버퍼: array[0...n-1]
    * `첫번째 await문`: 생산자와 소비자들이 각각 자신이 받은 번호값의 **순서대로** 진행하기 위해서
    * `두번째 await문`: 생산자의 경우 버퍼가 다 차 있을 때, 소비자의 경우 버퍼가 비어있을 때 기다려주는 것 
* 병행 프로세스 간의 상호배제와 동기화를 위한 기법들 
    * 단점:  **P와 V 명령 순서**를 바꾸거나 **advance 명령**을 제대로 하지 않았을 경우 -> 심각한 **오류** 발생 

## 모니터(Monitor)
* **`모니터`**: `공유 데이터`들과 이들에 대한 `임계영역`을 관리하는 `소프트웨어 구성체` 
    * `프로그래밍 언어 수준`에서 제공되는 `모듈` -> `공유 데이터`를 위한 `변수`들과 `초기화 루틴,` `임계영역을 코딩한 프로시저`들로 이루어진 일종의 `함(box)`
    * 모니터 내의 `변수`들은 `프로시저`들을 통해서만 접근이 가능
    * `프로세스`들은 모니터의` 프로시저`를 `호출`, 실행하여 `모니터` 안으로 진입한 후 원하는 `공유데이터`에 접근하게됨 
        * 언제나 모니터의 진입을 한 프로세스로 제한 -> 한 번에 하나의 `프로세스`만이 `모니터`에 있게함 -> **`상호배제`** 
* 모니터의 운영방식
    * 모니터로의 진입은 **프로시저 호출**로 가능 
    * 한 번에 한 프로세스만이 모니터에 들어갈 수 있음 -> 이미 한 프로세스가 들어가있을 때를 대비해 호출될 **프로시저 개수**만큼의 **대기 큐** 필요
    * 모니터내의 프로세스가 실행 중 특정 조건에 의해 대기해야할 경우 
        * 바로 자신은 모니터를 **양보**해서 밖의 다른 프로세스가 들어올 수 있게 함 -> **해당 조건의 큐**에서 기다려야함
        * 해당 조건을 만족하게 해주는 **프로세스**는 그 조건을 기다리던 프로세스의 진입으 위해 잠시 모니터를 비켜줌 이때 사용되는 큐를 **`신호자 대기 큐(Waiting Signaler Queue)`**
    * 모니터에서는 `조건 변수(Condition Varaible)`을 선언 
        * `모니터`에서만 선언하고 사용하는 것. `cwait()`와 `csignal()`에 의해서만 접근 가능 
    * `조건 대기`를 위해 `cwait()`
        * cwait(c): 이 연산을 호출한 `프로세스`를 `조건 c의 큐`에 대기시키고 다른 프로세스의 모니터 진입을 가능하게 함 
    * 대기에서 깨어나기 위해 `csignal()`
        * csignal(c): `cwait(c)에` 의해 대기되었던 프로세스를 `재개`시키고 자신은 `신호자 대기 큐`로 비켜줌 
        * cwait(c)로 대기중인 프로세스가 많다면 그 중 하나를 고름. 없다면 그냥 단순히 다음 문의 실행을 계속 
* 유한 버퍼에서 모니터를 사용한 생산자-소비자 알고리즘
    ```
        monitor boundedbuffer; //monitor 이름
        char buffer[n]; 
        int nextin,nextout,count; 
        cond notfull,notempty; //condition variables(조건변수들)
        void append(char x){ //producer를 위한 프로시저 append
            if(count == n) //buffer full 이다 
                cwait(notfull); 
            buffer[nextin]=x; 
            nextin=(nextin+1)%n; 
            count++; 
            csignal(notempty); //notempty라는 녀석이 조건 큐에 기다리고 있다면 깨워줌 
        }
        void take(char x){
            if (count==0) //buffer empty 이다 
                cwait(notempty); 
            x=buffer[nextout];
            nextout=(nextout+1)%n;
            count--; 
            csignal(notfull); 
        }
        {
            //monitor body
            nextin=0,nextout=0,count=0; //initialization 
        }
        ```
        ```
        void producer(){
            char x;
            while(true){
                create data x; 
                append(x); //모니터 안에서 단순히 프로시저를 호출하는 형태 -> error를 발생시킬 소지가 줄어듦 
            }
        }
        void consumer(){
            char x; 
            while(true){
                take(x); 
                consume data x; 
            }
        }
        void main(){
            parbegin
                producer(),consumer();
            parend 
        }
    ```
    * 생산자 프로세스 실행 과정 책 참고 
    * 호출할 수 있는 프로시저 개수만큼 진입 큐 존재 
    * 조건 수만큼 조건 큐 존재 
    * wait, signal은 진입큐, 조건큐 개수와 상관없이 언제나 각각 1개씩임
    * 생산자는 버퍼가 찼을 떄, 소비자는 버퍼가 비어있을 때 각각 조건 큐로 대기하게 되고, 데이터를 채우거나 비운 후에는 해당 조건 큐에 대기중인 프로세스를 재개시켜주는 방식으로 짜여져 있음 
    * 모니터가 비었을 때 진입 큐와 신호자 대기 큐에 있는 프로세스 중 누구에게 모니터 진입의 우선권을 주어야 하느냐? 
        * 신호자 대기 큐
        * 이유: 훨씬 이전에 실행되다가 다른일 때문에 대기중이던 프로세스이기 떄문이다 
* 스풀링(Spooling)을 위한 스풀러(Spooler/생산자) 및 디스풀러(De-Spooler/소비자)
    * 원형버퍼를 관리하는 모니터에서 응용되는 분야 
    * 스풀러 프로세스: 디스크와 같은 보조기억장치에 빠른 속도로 출력을 하는 프로세스
    * 디스풀러 프로세스: 디스크에 임시로 출력된 내용을 실제로 프린터로 출력하는 시스템 프로세스 
    * 상대적으로 속도가 느린 프린터와 직접적으로 관련된 일을 하는 디스풀러 프로세스는 스풀러 프로세스에 비해 처리속도가 느림 -> 속도 차이 완화 위해 원형버퍼를 사용 
* 상호배제와 동기화를 시스템이 알아서 다 해줄 수 있을까? 
    * 생산자-소비자 문제를 예로 들어서...
    * 상호배제: 모니터의 진입을 한 번에 한 프로세스로 제한함으로써 자연스레 해결 가능 
    * 동기화: 동기화 관련 부분이 모두 모니터 내에 표현되어있으므로 이것이 제대로 실행되는지 확인하고 오류를 발견하기도 쉬움 
    * 세마포어와 달리 잘 작동될 수 있음
        * 세마포어는 공유자원에 접근하는, 모든 프로세스들 중 하나라도 프로그램 이 잘못되면 오류를 일으킴 
* 식사하는 철학자 문제 
    * 철학자들은 비동기적으로 먹거나 생각하는 일들을 반복
    * 공유자원: 포크 -> 스파게티를 먹기 위해 자신의 왼쪽 포크와 오른쪽 포크가 필요됨 
    * 문제의 핵심: 철학자들의 정상적인 식사를 보장 
* 포크에 대한 상호배제를 위한 세마포어
    ```
    philosopher i:
    while (true){
        think; 
        P(fork[i]); 
        P(fork[(i+1)%5]); 
        eat; 
        V(fork[i]);
        V(fork[(i+1)%5]);
    }
    ```
    * 모든 철학자가 자신의 왼쪽 포크를 집고 난 후 오른쪽 포크를 집지 못해 교착상태(Deadlock)에 빠지는 철학자 발생 
* 한 번에 포크 두개를 동시에 집도록 하여 교착 상태 발생 방지
    ```
    philosopher i:
    while (true){
        think;
        P(fork[i] and fork[(i+1)%5]);
        eat;
        V(fork[i] and fork[(i+1)%5]);
    }
    ```
    * 문제: 두 개를 모두 집은 철학자는 식사가 가능하나, 하나를 못집은 철학자는 굶게 됨 -> 동작이 매우 빠른 철학자 때문에 오랜 시간 기아를 겪게되는 철학자 발생 가능 
        * 해결방법
            1. 식탁에 한번에 4명만 앉도록 함 or 홀수 번호의 철학자는 왼쪽 포크부터 집고난 다음 오른쪽 포크를 집도록 하고 짝수 번호의 철학자는 오른쪽 포크부터 집고난 다음 왼쪽 포크를 집도록 함 -> 교착 상태나 기아 없이 식사 가능 
            2. 모니터를 이용해 구현하면 교착 상태 방지 가능 
                * 한 명의 철학자만이 모니터 내에 있을 수 있으므로 왼쪽 포크를 집는 동안 다른 철학자가 오른쪽 포크를 집어갈 가능성 배제 가능  