# Crash Recovery

## Motivatioin
* recovery에서는 로깅을 함 
    * write하면 data 값만 write하는 것 뿐만 아니라 어떤 게 어디에 write를 했는지를 기록함 
    * 로깅 기법을 사용하면 Recovery Manager가 Log Manager 포함 
* t1,t2,t3는 이미 끝난 작업이므로 durability를 보장해줘야함
* t4, t5는 아직 안끝났으면 abort시켜야함 

## Recovery Strategy
* immediate update 정책 or deferred update 정책 
    * OS가 판단하여 사용하게 됨 
    * 장단점? 
1. Deferred(<->immediate) update 정책 
    * update할때마다 update를 물리적으로 하도록 하는 시스템은 거의 없음 -> 성능이 느리기 떄문 
    * response time을 높이기 위해 미뤄뒀다가 올림 
    * 만약 commit 전에 fail이 난다면 (t4,t5) -> update한다고 해도 db에 업데이트 되지 않음 (abort된것)
2. Immediate update
    * 바로 write를 실행 

## handling the buffer pool
* `Force`(=flush) 
    * 그러나 대부분 시스템은 no forcing system으로 감
* `Steal`: 버퍼를 steal한다 
    * *CPU 사이클 steal한다* 
    * 인터럽트 발생 시-> cpu를 잠시 빌린 후 인터럽트 처리 후 하던거 계속 함 
    * 성능을 높이기 위해 stealing을 하게 됨 
    * 트랜잭션을 동시에 돌리기 위해 서로 뺏거나 주거나 해야함 (공간이 필요)
        * 기존에 누가 쓰고 있던걸 뺏음 (buffer stealing 정책)
        * 그냥 뺏으면 안됨
        * 뺏긴 애는 기존에 하던거 저장하고 줘야함 
        * 버퍼를 다시 기존의 애에게 줄 수 있도록 백업을 디스크에 잘 해두어야 한다 
        * 커밋되지 않은 트랜잭션의 결과가 디스크에 가 있을 거임 -> 중구난방이 된다 -> atomicity가 까다롭게 되어버림 
    * 어쩔 수 없이 stealing은 필요함 

## Logging
* `REDO`: DO를 다시 하는 것 
    * 위의 t1,t2,t3를 guarantee 하기 위함 
        * t1,t2,t3가 했던 것을 다시 redo 하면 됨 
    * dbms가 했다는 것을 보고 '이거 했었다'
    * New value가 필요함 
        * DO operation: a를 10에서 20으로 바꿨다 
        * REDO 할 때 필요한 데이터 (a, 20) (new value: 20)
* `UNDO`: DO를 안한 것처럼 해주는 개념 (되돌아가기 위해서)
    * 트랜잭션의 atomicity를 보장하기 위함 
    * t4,t5는 UNDO를 해주어야함 
        * 실행하다 만 애들이므로 안한 것처럼 하기 위해 
    * old value가 필요함 
        * a의 이전 value는 10이었다는 남겨둬야함 
        * old value: 10 
* sequential write: 뭐를 DO, 뭐를 UNDO (순차적으로 기록되어야함)
    * 뭐하고 뭐가 같이 일어났다 -> X (Concurrency 기본)
    * 동시에 일어난 것 처럼 보이지만 실제로는 순서가 정해져있음(스케줄링)
* 가장 효율적으로 minimum한 정보(순차적인 정보)들만 남길 것이다 
* 로그가 어떤 정보를 저장하고, 그게 뭘 의미하는지를 공개하지 않음 
    * 보안성 측면 위해 
    * 효율성 위해 
* Log: An **ordered list** of **REDO/UNDO** actions
    * ordered list : 순차적, 정렬되어있다는 의미 
    * log record 형태: `<transactionID, data_item, old value, new value>` +  other info such as begin, and commit/rollback
        * REDO라면 new value가 차있을 거임
        * UNDO면 old value가 차있을 거임 
        * 트랜잭션이 언제 시작하고 끝났는지 기록
        * 트랜잭션이 커밋했는지 rollback했는지 기록 
            * 커밋한 트랜잭션이면 REDO
            * 아니면 UNDO
### Write-Ahead Logging (WAL)
* data를 write하기 전에 log 먼저 write되어야함 
    * db(disk)에 저장되기 전에 log가 먼저 저장되도록 해야함 
    * buffer에 기록된 log가 db로 들어가기 -> buffer에 기록된 변경된 테이블 데이터가 db로 들어가기 
    * 만약 transaction fail이 일어났을때 log가 기록되어있지 않다면 rollback할때 operation을 수행했다는 log가 기록되어있지 않으므로 rollback이 안됨 
    * transaction processing은 일(transaction)은 atomicity 보장하려면 log먼저 기록 하고 실제 disk의 data를 바꿔줘야한다!! 
* commit하기 전에 해당 transaction이 만들어 놓은 모든 log record는 다 write해야한다 
    * durability를 보장하기 위함 
    * commit하면 얘가 했던 모든 record가 쓰여져 있기 때문에 read만 하면 됨 -> 복구 가능 
* IBM의 ARIES (강의는 이 기반으로 설명된다)가 있음 
### Big Picture
* `log`를 통해 어떤 일을 해왔는지를 확인 가능 
    * checkpoint도 확인 가능 
    * t2가 commit했고, t4가 시작되엇고 등등의 정보를 log를 통해 알 수 있음
* t3, t5는 undone임 -> t3, t5가 한 operation은 `UNDO`해야함 (for **`atomicity`**!)
* t2, t4는 commit되었기 때문에 `REDO`해야함 (for **`durability`**!)
* T1은 checkpoint 이전에 끝났기(commit됨) 때문에 해줄 필요가 없음 
* 복구 과정: Analysis -> UNDO -> REDO
    * *자연스럽게* analysis한 다음에 UNDO(<-)를 하고 REDO(->)를 하도록 알고리즘이 짜여져있음 
### checkpointing
* checkpoint를 얼마나 주기적으로 할 것이냐는 어려운 일 (자동으로 안되고 관리자의 노하우로 해야함)
* Recovery할 때 시스템의 부담을 줄이기 위해 checkpointing을 함 