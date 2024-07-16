# Docker 이론

> 오늘은 실습보다 이론위주의 수업이었다.  
> 도커 보안에 대해 간단하게 설명하고 들었던 것들을 정리하고자 한다.  

## Docker Security
***
#### Rootless Mode
- 개념
  - Docker를 실행할 때 관리자 권한 없이 실행시키는 모드
- 특징
  - 시스템 전체에 대한 권한 없이 사용할 수 있어 잠재적 보안 위험을 낮출 수 있다.
  - 모두 일반 사용자 권한으로 시스템 자원에 대한 접근 제한

#### Linux Capabilities
- 개념
  - 리눅스 커널에서 제공하는 기능으로 시스템 관리 권한을 세분화하여 특정 작업에 대해 권한을 부여할 수 있다.
- 특징
  - 컨테이너 생성 시 필요한 권한만 부여해 보안성 향상
  - Privileged : 컨테이너가 호스트 시스템의 모든 권한을 갖고 있음.(보안 취약)

#### Share Namespace
- 개념
  - 리눅스 커널의 기능으로 프로세스와 자원(PID, 네트워크, IPC 등)을 격리 시킬 수 있다.
- 특징
  - Docker에서는 컨테이너 마다 별도의 네임스페이스를 할당해 다른 컨테이너나 호스트와 격리한다.
  - PID Namespace : 호스트의 프로세스를 볼 수 있음, 프로세스 ID를 격리하여 각 컨테이너가 다른 프로세스를 보지 못하게 한다.
  - IPC Namespace : 호스트의 , 인터이페스 통신을 격리한다.
  - Network Namespace : 호스트의 , 네트워크 인터페이스를 격리해 컨테이너 간 네트워크 충돌을 방지한다.
  - 추가적인 내용 [리눅스 심화](https://choiseungwoo98.github.io/linux_detail.html#namespace)

#### Security Layer
- 개념
  - 리눅스 커널에서 제공하는 보안 메커니즘으로 Docker 이미지에서 보안 패치를 적용한 레이어이다.
  - 시스템 호출 제어와 프로그램 동작 제한을 통해 보안을 강화하는데 사용
- 특징
  - Docker 이미지는 여러 레이어로 구성되며, 취약점 발견 시 해당 레이어만 업데이트 해 보안성 유지
  - 이미지 전체를 다시 빌드할 필요 없이 특정 레이어만 업데이트 할 수 있어 효율적이다.
  - AppArmor : 리눅스 커널에서 제공하는 보안 모듈로 애플리케이션 접근할 수 있는 자원을 제어(강제적 접근 제한 chmod, chown, setuid 등보다 더 로우한 거)
  - Seccomp : 리눅스 커널에서 제공하는 보안 기능으로 시스템 콜을 필터링해 애플리케이션을 실행할 수 있는 시스템 콜 제한 

<div style="height: 50px;"></div>

## Privileged Container와 UnPrivileged Container
***
#### Privileged Container
- 컨테이너는 Host에서 독립된 namespace 영역을 가지고 있어 시스템의 주요자원에 접근할 수 있는 권한이 없음
- Privileged 옵션으로 Container를 생성하면 Host의 리눅스 커널 기능을 모두 사용할 수 있고 모든 Host 주요 자원에 접근할 수 있음

#### UnPrivileged Container
- 시스템 주요 자원에 접근할 수 있는 권한이 부족해 네트워크 인터페이스 활성화/비활성화나 IP주소의 변경 불가능

<div style="height: 50px;"></div>

## Capabilities
***
- 모든 권한을 열어주게 되면, 보안적으로 위험할 수 있어 세분화하여 권한을 관리
- CAP_NET_ADMIN : 네트워크설정변경
- CAP_SYS_ADMIN : 시스템관리작업
- CAP_SYS_PTRACE : 다른프로세스추적

<div style="height: 50px;"></div>

## Reverse Shell
***
#### 개념
- 공격자가 타겟 시스템에 접속하여 원격으로 명령을 실행할 수 있도록 하는 공격 기법 중 하나
- 외부에서 내부로 접속을 가능하게 해 방화벽을 우회하는 데 사용

#### 작동 원리
- 공격자 준비 : 자신의 시스템에서 특정 포트를 열어 리스닝 상태로 대기(공격자 시스템)
- 타겟 시스템 접속 : 타겟 시스템에 악성 코드 또는 스크립트를 실행시켜, 시스템으로 연결(타겟 시스템에서 공격 시스템 연결)
- 연결 수립 : 연결을 통해 타겟 시스템에서 명령을 실행할 수 있는 쉘에 접근
- 명령 실행 : 공격자가 타겟 시스템에서 원하는 명령을 실행

#### 방어 방법
- 방화벽 설정 강화 : 내부에서 외부로의 불필요한 연결 제한, 중요한 시스템에서는 허용된 IP와 포트만 외부로 연결 설정
- 네트워크 모니터링 : 이상한 네트워크 트래픽을 감지하기 위해 네트워크 트래픽을 지속적으로 모니터링
- IDS/IPS 사용 : 침입 탐지 시스템(IDS)이나 침입 방지 시스템(IPS)을 사용하여 의심스러운 활동을 실시간으로 탐지 및 차단합니다.
- 보안 패치 적용 : 시스템과 애플리케이션의 보안 패치를 최신 상태로 유지하여 악성 코드가 실행될 수 있는 취약점을 최소화합니다.
- 사용자 교육 : 소셜 엔지니어링 공격을 통해 리버스 셸이 설치될 수 있으므로, 사용자에게 피싱 이메일과 같은 공격 방법에 대한 교육 실시

<div style="height: 50px;"></div>

## Linux root 디렉토리 정보
***
- /proc : 시스템 상태, 하드웨어 정보, 프로세스 정보 등이 포함
  - /proc/cpuinfo : CPU 정보
  - /proc/meminfo : 메모리 상태

- /etc : 시스템 및 애플리케이션의 설정 파일들이 저장
  - /etc/passwd : 사용자 계정 정보
  - /etc/fstab : 파일 시스템 마운트 정보
  - /etc/hosts : 호스트명과 IP 매핑 정보

- /bin : 시스템 부팅과 기본 운영에 필요한 실행 파일들이 저장
  - ls, cp, mv, rm 등의 명령어 위치

- /sbin : 시스템 관리와 관련된 명령어들이 위치 
  - reboot, ifconfig, iptables 등의 명령어가 포함

- /usr : 사용자 애플리케이션, 라이브러리, 헤더 파일 저장
  - /usr/bin : 일반 사용자 명령어
  - /usr/sbin : 시스템 관리자 명령어 위치
  - /usr/lib : 라이브러리 파일 포함

- /var : 로그 파일, 스풀 디렉토리, 캐시 파일 등이 포함 
  - /var/log : 로그 파일
  - /var/spool : 프린터와 메일 스풀 파일들이 저장

- /home : 각 사용자별 개인 파일과 설정 파일 저장
  - 사용자 user1의 홈 디렉토리는 /home/user1

- /tmp : 시스템 사용 중 임시로 생성되는 파일들이 위치, 시스템 재부팅 시 대부분의 파일이 삭제됨.

- /dev : 시스템 하드웨어와의 인터페이스를 제공하는 파일들이 포함
  - /dev/sda1 : 첫 번째 하드 디스크의 첫 번째 파티션을 나타냄

- /lib : 커널 모듈과 공유 라이브러리 파일들이 포함
  - /lib/modules : 커널 모듈 위치