# 파일 사용 

## 1. 파일 복사 
* `$ cp [-i] 파일1 파일2` :파일 1을 파일2에 복사한다. -i는 대화형 옵션 
    * ex> `$ cp cs1.txt cs2.txt`
* 대화형 옵션 : cp -i 
    * 복사 대상 파일과 이름과 같은 파일이 이미 존재하면 덮어쓰기 (overwrite)
    * 보다 안전한 사용법: 대화형 -i(interactive) 옵션 사용 
* `$ cp 파일 디렉터리` : 파일을 지정된 디렉터리에 복사한다.
* `$ cp 파일1 ... 파일n 디렉터리` : 여러 개의 파일들을 지정된 디렉터리에 모두 복사. 
* `$ cp [-r] 디렉터리1 디렉터리2`: r은 리커전 옵션으로 디렉터리1 전체를 디렉터리2에 복사한다. (하위디렉터리를 포함한 디렉터리 전체를 복사함.)


## 2. 파일 이동 
* `$mv [-i] 파일1 파일2`: 파일1의 이름을 파일2로 변경. -i는 대화형 옵션 
* 대화형 옵션: mv -i 
    * 이동 대상 파일과 이름이 같은 파일이 이미 존재하면 덮어쓰기 (overwrite)
    * 보다 안전한 사용법: 대화형 -i (interactive) 옵션 사용 
* `$mv 파일 디렉터리`: 파일을 지정된 디렉터리로 이동한다. 
* `$mv 디렉터리1 디렉터리2 `: 디렉터리1을 지정된 디렉터리2로 이름을 변경한다 



## 3. 파일 삭제 
* `$ rm [-i] 파일+` : 파일(들)을 삭제. -i는 대화형 옵션 
* `$rm [-ri] 디렉터리`: 디렉터리 전체 삭제. -r은 리커전 옵션. 디렉터리 아래 모든 것을 삭제. -i는 대화형 옵션


## 4. 링크 
* 링크: 기존 파일에 대한 또 하나의 새로운 이름 
    * `$ln [-s] 파일1 파일2`: 파일1에 대한 새로운 이름(링크)로 파일2를 만들어줌. -s는 심볼릭 링크. -> `하드링크`
    * `$ln [-s] 파일1 디렉터리`: 파일1에 대한 링크를 지정된 디렉터리에 같은 이름으로 만들어줌. -> `심볼릭 링크` 
* `하드 링크(hard link)`
    * 기존 파일에 대한 새로운 이름
    * 실제로 기존 파일을 대표하는 i-노드를 가리켜 구현 
* `심볼릭 링크(symbolic link)(=soft link)`
    * 다른 파일을 가리키고 있는 별도의 파일 
    * 실제 파일의 경로명을 저장하고 있는 일종의 특수 파일
    * 이 경로명이 다른 파일에 대한 간접적인 포인터 역할을 함 

## 5. 파일 속성
* 파일 속성
    * 블록 수(K 바이트 단위)
    * 파일 종류 (일반 파일 - , 디렉터리 d, 링크 l, 파이프 p, 소켓 s, 디바이스 b또는 c 등)
    * 접근권한 (파일에 대한 소유자, 그룹, 기타 사용자의 읽기r,쓰기w,실행x 권한)
    * 하드 링크 수 (파일에 대한 하드링크수)
    * 소유자 및 그룹 (파일의 소유자 id 및 소유자가 속한 그룹)
    * 파일 크기 (파일의 크기(바이트 단위))
    * 최종 수정 시간 (파일을 생성 혹은 최후로 수정한 시간)
* 파일 종류
    * 일반 파일: - 
        * 데이터를 갖고 있는 텍스트 파일 또는 이진 파일
    * 디렉터리 파일: d
        * 디렉터리 내의 파일들의 이름들과 파일 정보를 관리하는 파일 
    * 문자 장치 파일: c
        * 문자 단위로 데이터를 전송하는 장치를 나타내는 파일
    * 심볼릭 링크: l
        * 다른 파일을 가리키는 포인터와 같은 역할을 하는 파일
    * 기타 등등 

    * `$ file 파일`: 파일의 종류에 대한 자세한 정보 출력 


## 6. 접근 권한 (permission mode)
* 파일에 대한 읽기(r),쓰기(w),실행(x) 권한 
* 소유자(owner),그룹(group),기타(others)로 구분하여 관리 

* `$ chmod [-R] 접근권한 파일 혹은 디렉터리` : 파일 혹은 디렉터리의 접근권한을 변경. -R옵션을 사용하면 지정된 디렉터리 아래의 모든 파일과 하위 디렉터리에 대해서도 접근권한을 변경 

* 접근권한은 8진수로도 표현이 가능함. 
* 사용자 범위: u g o a 
* 연산자: + - =


## 7. 기타 파일 속성 변경 
* `$ chown 사용자 파일` / `$ chown [-R] 사용자 디렉터리` : 파일 혹은 디렉터리의 소유자를 지정된 사용자로 변경. -R 옵션은 디렉터리 아래의 모든 파일과 하위 디렉터리에 대해서도 소유자를 변경 
* `$ chgrp 그룹 파일` / `$ chgrp [-R] 그룹 디렉터리`: 파일 혹은 디렉터리의 그룹을 지정된 그룹으로 변경. -R 옵션을 사용하면 지정된 디렉터리 아래의 모든 파일과 하위 디렉터리에 대해서도 그룹을 변경. 
* `$ touch 파일` : 파일의 최종 사용 시간과 최종 수정 시간을 현재 시간으로 변경. 
