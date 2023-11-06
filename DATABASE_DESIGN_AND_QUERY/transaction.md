# Transaction

## Single user vs Multi user Systems
* Interleaved processing
* parallel processing

## Transactions
* Usability(유용성): database에 concurrent 해야함 
* logical unit

## Concurrency in a DBMS
* 각각의 transaction은 database를 consistent한 상태로 남겨둬야함 
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