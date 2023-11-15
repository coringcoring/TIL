# Concurrency Control

## Deadlocks
* 시스템이 deadlock을 신경써줘야하는 이유: 트랜잭션이 서로 자원을 가지고 놔주지 않아서 더 이상 어떠한 일도 할 수 없게 되므로 
* deadlock을 handling하는 법
    1. prevention: deadlock을 아예 일어나지 않도록 하는 것 
        * timestamp에 따라 우선순위를 두기 
            * Wait-Die
            * Wound-wait 
    2. detection : deadlock이 일어난 것을 찾아서 해결함
        * **`waits-for graph`**를 만들자 
                * node: transaction
                * edge: 누가 누구를 기다리고 있는지 

## Multiple-Granularity Locks
* granularity를 어떻게 정할지 딱 정하는게 어려움 -> 새로운 lock 등장 `intention lock`
    * intention: ~할 의도가 있다 
    * IS lock: S할 의도가 있다
    * IX lock: X할 의도가 있다 
    * SIX lock ((I)S lock과 IX lock을 동시에 걸리고/해제되는 것)
        * SIX와 X는 comparable한가? 도 생각하자.. (IS,IX,X 모두 고려) 





