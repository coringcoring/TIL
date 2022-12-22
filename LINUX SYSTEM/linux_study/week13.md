# 프로세스 원리 

## 프로세스 이미지 
* 프로세스: 실행(프로그램의 코드, 데이터, 스택, 힙, U-영역 등 필요) 중인 프로그램  
    * 프로세스 이미지(구조): 메모리 내의 프로세스 레이아웃 
    * 프로그램 자체가 프로세스는 아님 
* `프로세스 이미지` 
    * 텍스트(코드): 프로세스가 실행하는 실행 코드를 저장하는 영역 
    * 데이터: 프로그램 내에 선언된 변수(global vairable) 및 정적 변수(static variable) 등을 위한 영역 
    * 힙: 동적 메모리 할당을 위한 영역
    * 스택: 함수 호출을 구현하기 위한 실행시간 스택(runtime stack)을 위한 영역
    * U-영역: 열린 파일의 파일 디스크립터, 현재 작업 디렉터리 등과 같은 프로세스의 내부 정보 
* size 명령어 : `$ size [실행파일]` 
    * 실행파일의 각 영역의 크기를 알려줌. 실행파일을 지정하지 않으면 a.out을 대상으로 함. 

## 프로세스 ID
* 프로세스 id
    ```c
    #include <unistd.h>
    int getpid();  //프로세스 id 반환
    int getppid(); //부모 프로세스의 id 반환 
    ``` 
    ```c
    #include <stdio.h>
    #include <unistd.h>
    #include <stdlib.h>

    int main(){
        printf("Hello !\n"); 
        printf("나의 프로세스 번호 : [%d] \n", getpid()); 
        printf("내 부모 프로세스 번호: [%d] \n", getppid()); 
        system("ps"); //프로그램 내에서 리눅스 명령어(ps 명령어) 실행시킴 
    }
    ``` 

## 프로세스 생성 
* `fork() 시스템 호출` : 부모 프로세스를 똑같이 복제하여 새로운 자식 프로세스를 생성 (`자기복제`)
    ```c
    #include <unistd.h>
    pid_t fork(void); 
    //새로운 자식 프로세스를 생성. 자식 프로세스에게는 0을 반환, 부모 프로세스에게는 자식 프로세스 id 반환 (한번 호출되면 두 번 return) -> 부모 프로세스와 자식 프로세스는 병행적으로 각각 실행을 계속함 
    ``` 
* 프로세스 기다리기: wait()
    ```c
    #include <sys/types.h>
    #include <sys/wait.h>

    pid_t wait(int *status); 
    //자식 프로세스 중의 하나가 종료할 때까지 기다림. 자식 프로세스가 종료하면 종료코드가 *status에 저장. 종료한 자식 프로세스의 id를 반환 
    ```
## 프로그램 실행 
* 프로그램 실행
    * fork 후 : 자식 프로세스는 부모 프로세스와 똑같은 코드 실행 
    * 자식 프로세스에게 새로운 프로그램 실행 시키기 : `exec()` 시스템 호출 사용 
        * 프로세스 내의 프로그램을 새 프로그램으로 대치 
        * `exec()` : 프로세스 내의 프로그램은 완전히 새로운 프로그램으로 대치, 새 프로그램의 main()부터 실행 시작 -> `자기대치`
            * exec()호출이 성공하면 return할 곳이 사라짐 
            * 성공한 exec() 호출은 절대 리턴하지 않음 (실패하면 -1 반환)
            ```c
            #include <unistd.h>
            int execl(char* path, char* arg0, char* arg1, ..., char* argn,NULL); 
            ```
            
            ```c
            #include <stdio.h>
            #include <unistd.h>

            /*echo 명령어 실행*/
            int main(){
                printf("시작\n"); 
                execl("/bin/echo","echo","hello",NULL);
                printf("exec 실패!\n"); 
            }
            ``` 
## 프로그램 실행 과정 

## 시스템 부팅 