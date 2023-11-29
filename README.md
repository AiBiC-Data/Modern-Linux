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

