# Modern-Linux

# 리눅스 커널

## 리눅스 아키텍처

- **하드웨어 계층**
    - CPU, 메인 메모리, 디스크 드라이브, 네트워크 인터페이스, 키보드, 모니터 등
- **커널 계층**
    - CPU인터페이스
    - 메모리와의 인터페이스
    - 파일시스템과 블록 디바이스 드라이버 인터페이스
    - 네트워크 인터페이스와 드라이버
    - 입력 디바이스를 위한 디바이스 드라이버
- **사용자 영역 계층**
    - 셸과 같은 운영체제 구성요소, ps나 ssh 같은 유틸리티, 그래픽 사용자 인터페이스를 비롯해 대부분의 앱이 실행되는 곳

커널과 사용자 영역 사이에는 **시스템콜(system call)**이라는 인터페이스가 있다. 

하드웨어와 커널 사이에는 그룹화 된 개별 인터페이스 모음으로 구성

![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/0aaf222d-d2e6-4bef-a324-b3baad475469)


일반적으로 커널 모드는 추상화를 제한함으로써 빠르게 실행함

사용자모드는 더 안전하고 편리한 추상화 의미

## CPU 아키텍처

- BIOS와 UEFI
    - 유닉스와 리눅스는 전통적으로 자체 부트스트랩을 위해 **BIOS**를 사용. 모던 환경에서는 이러한 BIOS 기능이 운영체제와 플랫폼 펌웨어 간의 소프트웨어 인터페이스를 정의하는  **UEFI(통일 확장 펌웨어 인터페이스)로 대체**.
        - CPU 확인 ⇒ `lscpu`
            
            ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/1c8e5489-aed3-4ce3-a28c-712d94fe3680)
            
        - CPU 확인2 ⇒ `cat /rpoc/cpuinfo` (CPU 개별로 전부 출력됨)
            
            **processor** : 현재 CPU의 인덱스 번호. 멀티코어 프로세서 시스템에서 각 코어에 대한 정보가 별도로 출력
            
            **vendor_id** : CPU 제조사를 식별하는 문자열. "GenuineIntel"은 인텔, "AuthenticAMD"는 AMD를 의미.
            
            **cpu family** : CPU가 속한 제품군. 예를 들어, 인텔의 i3, i5, i7 AMD의 Ryzen 3, Ryzen 5, Ryzen 등이 있다.
            
            **model** : CPU 모델 번호. 특정 CPU 모델을 구분하는 데 사용.
            
            **model name** : CPU의 모델 이름. 예를 들어 11th Gen Intel(R) Core(TM) i7-1165G7 @ 2.80GHz 처럼 흔히 CPU하면 확인하는 정보를 출력.
            
            **stepping** : CPU 스테핑 번호. 스테핑은 CPU 제조 과정에서 개선 사항을 반영한 버전을 의미.
            
            **microcode** : CPU의 마이크로코드 버전. 마이크로코드는 CPU 내부에서 실행되는 작은 프로그램.
            
            **cpu MHz** : CPU의 클럭 속도. 메가헤르츠 단위로 표시.
            
            **cache size** : CPU 캐시의 크기. 일반적으로 L2 또는 L3 캐시가 표시. 각 캐시 레벨의 크기를 정확하게 확인하려면 **lscpu** 명령어를 사용할 수 있다.
            
            **physical id** : 물리적 CPU 번호. 멀티소켓 시스템에서 각 프로세서에 대한 정보를 구분하는 데 사용.
            
            **siblings** : 윈도우로 치면 논리 프로세서와 같은 개념. 물리적 CPU에 속한 코어의 총개수를 의미. 인텔의 경우 하이퍼스레딩이 활성화된 경우 이 코어 수가 물리 코어보다 더 많다.
            
            **core id** : 현재 코어의 고유 식별자.
            
            **cpu cores** : 물리적 CPU 코어 수.
            
            **apicid / initial apicid** : CPU의 고유한 APIC 식별자. APIC는 중앙 처리 장치와 입출력 장치 간의 인터럽트를 관리하는 시스템을 의미.
            
            **fpu** : 이 값은 부동 소수점 계산을 지원하는지 여부. 일반적으로 대부분의 최신 CPU에서는 yes로 표시.
            
            **fpu_exception** : 부동 소수점 예외를 지원하는지 여부. 이는 CPU가 부동 소수점 연산에서 예외 처리를 지원하는지를 표시.
            
            **cpuid level** : CPUID 명령어를 사용할 때 얻을 수 있는 최대 정보 수준을 나타냅니다.
            
            **wp** : Write Protection(쓰기 보호) 기능이 활성화되어 있는지 여부.
            
            **flags** : CPU가 지원하는 기능과 명령어 집합을 나열한 목록. 예를 들어, sse, sse2, avx 등의 SIMD 확장 명령어와 가상화 기술을 지원하는지 여부를 나타내는 vmx 또는 svm 등이 포함될 수 있다.
            
            **bogomips** : CPU의 성능을 대략적으로 측정하는 데 사용되는 BogoMIPS라는 단위. BogoMIPS는 커널이 부팅 과정에서 시스템 타이밍을 설정하는 데 사용되는 가상의 성능 척도. 이 값은 실제 CPU 성능과는 관련이 없다.
            
            **clflush size** : CPU에서 지원하는 캐시 라인 크기. CLFLUSH 명령어는 캐시에서 지정된 캐시 라인을 플러시(비우기)하는 데 사용.
            
            **cache_alignment** : CPU 캐시의 정렬 값. 일반적으로 캐시 정렬이 잘 되어 있을수록 성능이 향상.
            
            **address sizes** : CPU가 지원하는 물리적 및 가상 주소 크기를 나타냅니다. 이는 시스템에서 사용할 수 있는 메모리 크기와 주소 공간을 결정. 예를 들어, 36 bits physical, 48 bits virtual은 물리적 주소 크기가 36비트이고 가상 주소 크기가 48비트.
            
            **power management** : CPU에서 지원하는 전력 관리 기능을 나열한 목록. 예를 들어, acpi, apm 등의 전력 관리 기술이 포함될 수 있다.
            
            ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/8b1a662d-986a-440f-a025-720e90ee9f4e)
            

## 커널 구성요소

- **프로세스 관리**: 실행 파일을 기반으로 프로세스를 시작함
    
    **프로세스** : 프로그램을 메모리 상에서 실행 중인 작업
    
    **스레드** : 프로세스 안에서 실행되는 여러 흐름 단위(프로세스마다 최소 1개의 스레드 소유)
    
    **프로세스 그룹**: 하나 이상의 프로세스가 포함돼 있으며 한 세션에는 포어그라운드 프로세스 그룹이 둘 이상일 수 없다. 
    
    **세션**: 사용자가 로그인해 작업하는 터미널(terminal) 단위로 프로세스 그룹을 묶은 것. 커널은 세션ID(SID)라는 번호를 통해 세션을 식별
    
    **태스크**: task_struct라는 데이터 구조가 있으며 프로세스와 스레드 구현의 기반을 형성
    
    ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/0c5f6846-839b-4c80-982e-16fee7cb2cca)
    
    (⇒ 같은 터미널에서 실행되어서 SID는 같은 것을 확인할 수 있고, 각각의 프로세스는 자신의 PID와 PGID가 같음을 알 수 있다.)
    
    ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/b6375167-17ee-4df4-a08e-979315bba9b5)
    
    ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/7d2e79de-4612-4e54-9ae4-338f45fca1b3)
    
- **메모리 관리**: 프로세스에 메모리를 할당하거나 파일을 메모리에 매핑함
    
    가상 메모리는 프로세스가 실제 메모리의 크기와 상관없이 메모리를 이용할 수 있도록 지원하는 기술. 실제 메모리(RAM, main memory, first storage)와 보조 기억 장치(auxiliary storage, secondary storage)의 Swap 영역으로 구성
    
    OS 는 메모리 관리자(Memory Management Unit)를 통해 메모리를 관리하며 프로세스는 사용하는 메모리가 실제 메모리인지, Swap 영역인지 모름.
    
    **TLB**: 가상 메모리 주소를 물리적인 주소로 변환하는 속도를 높이기 위해 사용되는 캐시
    
    ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/49c2e629-e4a0-4dc7-bd87-9071deb2240b)
    
    (1. 가상주소가 주어지면, 처음에 TLB 살펴봄. 2-1. 가상 주소가 TLB에 존재→TLB hit→바로 프레임 번호 추출→ 실주소 구성 2-2. 해당 페이지테이블 항목 부재→TLB miss→ 페이지 번호로 페이지 테이블 인덱싱→페이지 테이블 항목 참조 3-1. 존재비트 1일 경우, 해당 페이지 주기억장치에 존재→ 페이지테이블 항목의 프레임 번호 이용→실주소 구성 및 TLB 갱신 3-2. 존재비트 0일 경우, 해당 페이지 주기억장치에 존재X→메모리 접근 오류 발생(page fault)
    
    ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/332e72d0-283d-4e68-a137-84a39eae0045)
    
    `grep MemTotal /proc/meminfo`: 물리적 메모리(RAM)에 대한 세부정보
    
    `grep VmallocTotal /proc/meminfo`: 가상 메모리에 대한 세부 정보
    
    `grep Huge /proc/meminfo`: 페이지 정보 나열
    
- **네트워킹**: 네트워크 인터페이스 관리나 네트워크 스택 제공함
    
    소켓: 추상화 커뮤니케이션을 위해 필요
    
    전송 제어 프로토콜(TCP) 및 사용자 데이터그램 프로토콜(UDP): 각각 연결형 통신과 비연결형 통신
    
    인터넷 프로토콜(IP): 기기의 주소 지정을 위해 필요;
    
- **파일시스템**: 파일 관리를 제공하고, 파일 생성과 삭제를 지원함
    
    HDD,SSD, 플래시 메모리 같은 저장 디바이스의 파일과 디렉터리 구성
    
    ext4, btrfs, NTFS 등의 유형의 파일 시스템 존재
    
- **캐릭터 디바이스와 디바이스 드라이버 관리**
    
    드라이버는 커널에서 실행되는 코드
    
    드라이버는 커널에 정적으로 빌드 될 수도 있고, 필요할 때 동적으로 로드될 수 있도록  커널 모듈로 빌드 될 수도 있다.
    
    `ls -al /sys/devices/` : 리눅스 시스템 디바이스의 개요 확인
    
    `mount:` 마운트된 디바이스 나열
    

## 시스템 콜

시스템 콜 실행 단계

1. syscall.h와 아키텍처 종속 파일에 정의되어 메모리에 있는 함수 포인터 배열을 통해 시스템 콜과 해당 핸들러를 추적
2. 시스템 콜 멀티플렉서처럼 동작하는 system_call()함수를 사용하면 먼저 하드웨어 컨텍스트를 스택에 저장한 다음 검사를 수행하고, 그 이후에 sys_call_table의 각 시스템 콜 번호의 인덱스가 가리키는 함수로 점프
3. sysexit로 시스템 콜이 완료되면 래퍼 라이브러리는 하드웨어 컨텍스트를 복원하고, 프로그램 실행은 사용자 영역에서 다시 시작된다.

![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/7d11e9ec-64c2-4423-871f-0d9ddfa96955)

strace ls를 사용하면 strace에 ls가 사용하는 시스템 콜을 캡처하도록 요청한다.( 내용이 많음)

execve 시스템 콜은 /usr/bin/ls를 실행해 셸 프로세스를 교체

brk 시스템 콜은 메모리를 할당하는 오래된 방식. malloc이 더 좋다.

access 시스템 콜은 프로세스가 특정 파일에 접근할 수 있는지 확인

등

# 셸과 스크립팅

## 기본개요

- **터미널**
    - 텍스트로 된 사용자 인터페이스를 제공하는 프로그램 → 텍스트를 읽어 화면에 표시하는 기능
- **셸**
    - 터미널 내부에서 실행되며 명령 인터프리터 역할을 하는 프로그램
    - 입출력 처리, 변수 지원, 명령 실행 및 상태 처리, sh로 정의하며 bash셸이 기본
        
        ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/eefca27f-83db-46df-bf21-e76319f42bb5)

        
- **셸의 기본 기능**
    - 스트림
        - 입출력을 위한 세가지 기본 파일 디스크립터(FD)를 모든 프로세스에 제공 → (stdin, stdout, stderr)
        - 특별하게 지정하지 않는 한 키보드에서 입력을 가져오고 출력을 화면에 전달
        - & ⇒ 명령 마지막에 배치되며 백그라운드에서 명령을 실행
            
            ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/72674f9f-525e-49bc-814e-60e4a08d47da)

            
        - \ ⇒ 긴 명령의 가독성을 높이기 위해 다음 행에서 명령을 계속할 때 사용
            
            ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/e67174e3-4a58-426d-b795-d96c3d78d494)

            
        - | ⇒ 한 프로세스의 stdout 값을 다음 프로세스의 stdin과 연결해 데이터를 파일에 임시로 저장하지 않고 바로 전달
            
            ![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/7b8358a3-2957-456a-8b3f-410757498c40)

            
    - 변수
        - 리눅스가 노출하는 구성 항목을 처리하려는 경우
        - 스크립트에서 사용자에게 값을 대화 형식으로 질문하려는 경우
        - 긴 값을 한 번 정의해 입력을 줄이려는 경우
        - 종류
            - 환경 변수 ⇒ 셸 전체 설정. env 명령어로 목록을 나열
            - 셸 변수 ⇒ 현재 실행 상황에 유효, 하위 프로세스는 셸 변수 상속X
        - 배시에서 export 명령어를 사용해 환경변수를 만들 수 있다. 변수의 값에 접근하고 싶을 때는 앞에 $를 붙이고 변수를 제거하고 싶을 때는 unset 사용
            
            ```bash
            $ set MY_VAR=42      # MY_VAR이라는 셸 변수슬 생성하고 값을 42로 지정 
            $ set | grep MY_VAR  # 셸 변수를 나열하고 MY_VAR 필터링. _= ->환경 변수로 내보내지 않음
            _=MY_VAR=42
            
            $ export MY_GLOBAL_VAR = "fun with vars" # 새 환경변수 생성
            
            $ set | grep 'MY_*'  # 셸 변수 나열하고 MY_로 시작하는 변수들 필터링
            MY_GLOBAL_VAR='fun with vars'
            _=MY_VAR=42
            
            $ env | grep 'MY_*'  # 환경 변수 나열
            MY_GLOBAL_VAR=fun with vars
            
            $ bash               # MY_VAR 상속하지 않는 현재 세션의 자식 프로세스 생성
            $ echo $MY_GLOBAL_VAR
            fun with vars
            
            $ set | grep 'MY_*'  # MY_GLOBAL_VAR만 나온다
            MY_GLOBAL_VAR='fun with vars'
            
            $ exit      # 자식 프로세스 종료 후 MY_VAR 셸 변수 제거하고 나열
            $ unset $MY_VAR
            $ set | grep 'MY_*'
            MY_GLOBAL_VAR='fun with vars' 
            ```
            
    - 종료 상태
        - 명령 실행 완료를 명령 호출자에게 종료 상태를 이용해 알린다. 리눅스 명령은 종료 될때 상태를 반환하고 정상적으로 종료되면 0, 비정상 종료일 경우 1~255 사이의 값으로 실패를 나타낸다.
    - 내장 명령어
        - help를 통해 내장 명령어 목록을 나열 할 수 있고 그 외의 명령어들은 /usr/bin이나 /usr/sbin에 있는 셸 외부 프로그램.
    - 작업 제어
        
        명령어를 입력하면 그 명령은 일반적으로 화면과 키보드를 제어하며 포어그라운드에서 실행. 백그라운드에서 실행하려면 마지막에 & 넣고, 셸을 닫은 후에도 백그라운드 프로세스를 계속 실행하려면 nohup 명령을 앞에 추가. 실행중인 프로세스를 제거하려면 강제성과 함께 kill 명령 사용.
        

## 모던 리눅스 명령어(참고)

- exa로 디렉터리 내용 나열
    - Rust언어로 개발되어 ls 대체, 색일 들어가서 보기 좋아지고 몇가지 기능 더해짐
    - [https://www.lesstif.com/lpt/directory-ls-exa-119963840.html](https://www.lesstif.com/lpt/directory-ls-exa-119963840.html)
- bat로 파일 내용 보기
    - cat 대체, 구문을 강조 표시 할 수 있고 인쇄할 수 없는 문자 보여주고 깃 지원
    - [https://www.lesstif.com/lpt/cat-linux-bat-119963780.html](https://www.lesstif.com/lpt/cat-linux-bat-119963780.html)
- rg로 파일에서 컨텐츠 찾기
    - find + grep 대체
    - [https://www.lesstif.com/lpt/grep-ripgrep-119963754.html](https://www.lesstif.com/lpt/grep-ripgrep-119963754.html)

## 일반 작업

diff - 파일 비교, head - 상위 몇 줄 출력, tail - 하위 몇 줄 출력

## 터미널 멀티플렉서

 **tmux** 또는 **screen**을 이용하여 **세션을 유지(**screen은 이제 잘 안 쓰임)

**<tmux 실행 순서>**

| 구분 | 명령어 |
| --- | --- |
| tmux 실행 | tmux  |
| srun 등의 작업 실행 |  |
| tmux 세션 유지하고 나가기 | ctrl + b 누르고 d키 누르기 |
| 특정 이름으로 세션 만들기 | tmux new -s 세션이름 |

**<tmux 세션 다시 접속하기>**

| 구분 | 명령어 |
| --- | --- |
| 세션 리스트 확인 | tmux ls |
| 세션 attach | tmux attach-session -t 세션번호 |
| 세션 kill | tmux kill-session -t 세션번호 |

![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/cb7f7798-40e8-4c99-b851-7c2f9f95a286)


![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/43e2f397-fff5-4e33-a85e-fa4a90fa66bd)


![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/2eb5dc29-22e5-4ef9-a5e6-e7fb5608fe9b)


## 스크립팅

### 기본 개요

- 고급 데이터 유형
    - 셸은 일반적으로 모든 것을 문자열로 취급하지만 배열과 같은 일부 고급 데이터 유형은 지원한다.
        
        ```bash
        	os=('Linux' 'macOS' 'Windows'  # 요소가 3개인 배열 선언
        echo "${os[0]}"  # 첫번째 요소에 접근
        numberofos="${#os[@]}"  # 배열의 길이를 가져온다.
        ```
        
- 흐름 제어
    - 스크립트에서 분기(if) 또는 반복(for와 while)을 통해 특정 조건에 따라 실행되게 할 수  있다.
        
        ```bash
        for afile in /tmp/* ; do  # 디렉터리를 반복하며 각 파일명을 출력하는 루프
        	echo "$afile"
        done
        
        for i in {1..10}; do  # 범위 루프
        	echo "$i"
        done
        
        while true; do  # 무한 루프 (ctrl+c로 탈출)
        	...
        done
        ```
        
- 함수
    - 셸은 스크립트를 위에서 아래로 해석하므로 함수는 미리 정의한 뒤에 사용
- 고급 I/O
    - read를 사용하면 런타임 입력을 유도할 때 사용할 수 있는 stdin에서 사용자 입력을 읽을 수 있다. 또 echo 대신 printf를 사용하는 것도 좋다.

### 작성

텍스트 파일은 첫 번째 행에서 **#!로 작성하는 shebang**을 사용해서 선언해야 한다.

chmod+x로 권한을 변경해주어야 한다.(chmod 750이 좋다)

### 우수 사례

errexit과 pipfail 등을 이용한다

암호같은 밑감 정보 하드코딩 X

가능하다면 변수에 정상적인 기본값을 설정, 제공하고 사용자나 기타 소스로부터 받은 입력을 깔끔하게 정리

의존성을 확인한다.

에러 처리를 할 수 있도록 실행할 수 있는 지침을 제공

메인 블럭의 스크립트를 인라인으로 문서화& 가독성 높이기

버전 관리

스크립트를 린트하고 테스트

### 예제 - 깃허브 사용자 정보 스크립트

![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/6e7ae5e8-e618-4daa-b956-cca0fc19e4d3)


![image](https://github.com/AiBiC-Data/Modern-Linux/assets/76275193/df8ee9b4-1f0e-493e-b4ff-3c24e7e0cb00)

