# Transaction

## Single user vs Multi user Systems
* **Interleaved processing**
* parallel processing

## Transactions
* Usability(유용성): database에 concurrent 해야함 
* Performance: disk 접근 속도가 상대적으로 느리기 때문에, 다수의 사용자 프로그램이 동시적으로 작동할 수 있도록 cpu humming을 유지하는 것이 중요 
* `transaction`: 사용자 프로그램의 DBMS의 추상화된 개념. 연속적으로 읽고, 쓰는 것을 포함하는 데이터베이스 처리의 logical unit

## Concurrency in a DBMS
* Concurrency 는 DBMS에 의해서 수행됨 
    * R, W 를 섞음
* 각각의 transaction은 database를 consistent한 상태로 남겨둬야함 
    * DBMS는 CREATE TABLE에 선언된 대로 ICs(Integrity Constraint)를 enforce할 것임 
    * 잘못 섞어서 processing하면 문제가 될 수 있음 
    * issue: effect of interleaving transactions, crashes 

## Anomalies with interleaved execution
* The Lost Update Problem
* Reading Uncommitted Data ("dirty reads")
    * dirty read: 상대방이 쓰는걸 덜 끝냈는데 중간에 읽는 것
    * T1 실행을 취소함 
    * T2는 "uncommited value" of X를 읽음 -> dirty read  
* Incorrect summary 
    * 여기서 sum은 그냥 local 변수임 
    * data의 상태는 맞으나 sum의 결과가 잘못됨 (100이 나와야 하는데 50임..)
* DBMS는 다수의 transaction들을 동시적으로 처리가 되어야한다. 
    * user의 usability와 performance 때문에 
* 여러개의 transaction들의 수행은 interleaved될 수 있다
    * 그러나 interleaving의 수행은 anomaly들을 일으킬 수 있음 
    * 따라서 이러한 anomaly를 방지하기 위해 concurrency control을 해야함 

## transaction의 원자성/ Recovery는 DBMS에 필요됨 
* 모든 action을 수행하거나, action들을 하나도 실행 안하거나 
* 다양한 형태로 failure들이 발생할 수 있음 -> recovery system이 DBMS에 요구됨 
    * DBMS는 모든 활동들을 기록(log)하기 때문에 non-commited한 transaction들의 활동을 undo할 수 있음 

## Transaction states와 transaction diagram
* active
* commited
* aborted (=roll back)
* terminated

## ACID Properties of Transaction
* transaction을 처리하는 도중의 consistency를 보장하는 것은 신경쓰지 않음. 끝난 상태가 consistency를 유지하고 있으면 상관없음 
1. Atomicity
2. Consitency
3. Isolation
4. Durability 

## Schedule of Transactions and Serializability 
* transaction들이 하나씩 seraialize한 것처럼 수행되도록 하자 (concurrent 실행이 아님! concurrent와 반대인 serialize) -> **`serializable schedule`** 
    * `serialize` : 순차 수행 <-> concurrent: 동시 수행 

## schedule of transactions and serializable schedule
* serial schedule (순차 수행)
* serializable schedule: 트랜잭션을 serial execution한 어떤 것들과 같기만 하면 serializable schedule라 할 수 있음 

## Conflict Serializable Schedules
* Conflict equivalent(충돌적 동일성) 뜻: conflict 나타나는 것들이 순서가 같다 
* conflict serializable: 스케줄이 몇몇 serial schedule에 conflict equivalent할 때 (어떤 serial과 conflict 측면에서 같은 것!)