# [형상관리] 형상관리란 SVN? GIT?

> 현재 가장 많이 사용하고 있는 형상관리도구 git에 대해 공부를 하다가 문득 궁금한 것이 생겼다.
> Git은 형상관리 도구 중 하나인데 다른 형상관리도구랑 어떠한 차이가 있을까?
> 그래서 한번 각각의 형산관리 도구에 대해 알아보고 장단점은 무엇인지 알아보고자 한다.

## 형상관리??
***
1. **소프트웨어 구성 관리(Software Configuration Management)** 또는 **형상관리**는 소프트웨어의 **변경사항을 체계적으로 추적하고 통제** 하는 것  
2. 일반적으로 단순 버전관리 기반의 소프트웨어 운용을 좀 더 포괄적인 학술 분야의 형태로 넓히는 근간  
3. 소프트웨어의 **소스 코드, 개발 환경, 빌드 구조 등 전반적인 환경 전반적인 내역**에 대한 관리 체계를 정의

형상 관리는 포괄적인 개념, 통상적으로 버전관리, 소스관리 등으로 불립니다. 즉, 정보를 여러 버전을 관리하는 것

<div style="height: 50px;"></div>

## 변경관리 / 버전관리 / 형상관리
***
변경관리, 버전관리, 형상관리. 표면적인 의미로 보면 거의 비슷하지만 이들은 제어 및 지원 범위에서 차이가 있다.
- 변경 관리 : 소스코드 변경 사항에 대한 관리  
- 버전 관리 : 변경사항을 ‘버전’이란 개념을 통해 관리.
- 형상 관리 : 위의 개념을 포함해 프로젝트와 관련된 모든 변경사항을 관리.

포함관계를 포함하자면  
**변경관리 ⊆ 버전관리 ⊆ 형상관리**
<details>
    <summary style="color: rgba(113, 187, 222, 1); cursor: pointer;">형상관리</summary>
    <img src="/img/posts/형상관리/형상관리란.png">
</details>
<details>
    <summary style="color: rgba(113, 187, 222, 1); cursor: pointer;">3가지 버전</summary>
    <img src="/img/posts/형상관리/3가지_관리_사진.jpg">
</details>

<div style="height: 50px;"></div>

## 버전 관리 시스템?
***
형상관리 중에서 **문서, 소스코드 등을 버전을 관리**해주는 버전관리시스템이다.  
통상적으로 **"형상관리 ≒ 버전관리"** 임을 인지하고 접근하는 것이 이해하기 쉽다.  

<div style="height: 50px;"></div>

## 버전 관리(형상 관리)를 위한 도구와 특징
***
CVS(Concurrent Version System)
- 90년에 출시된 무료 서버-클라이언트 형상관리 시스템
- 파일 전체를 저장하는 것이 아니라 변경사항만을 저장
- 변경사항만 저장하기 때문에 용량을 적게 차지하지만 속도가 상대적으로 느림

Perforce(P4D)
- 빠른 속도, 빠른 Merge가 가능하며 큰 리소스 관리에 좋음
- 유료이고 파일명이 바뀌면 히스토리 추적이 곤란

SVN (Subversion)
- 형상관리/소스관리 툴의 일종
- 중앙관리만 지원
- 다른 사용자의 커밋과 엉키지 않으며, 커밋 실패 시 롤백 기능을 지원

Git
- 분산형 버전관리 시스템
- Repository의 완전한 복사본을 로컬에 저장할 수 있음
- 처리속도가 빠르지만 대용량 코드 관리에 부적절


<div style="height: 50px;"></div>

## 가장 많이 사용하는 SVN과 Git 비교
SVN (Subversion)
- SVN은 보통 대부분의 기능을 완성해놓고 **소스를 중앙 저장소에 commit**
- commit의 이미 자체가 중앙 저장소에 해당 기능을 공개한다는 의미.
- 개발자가 자신만의 **version history를 가질 수 없음**.
- **commit한 내용에 실수가 있을 시에 다른 개발자에게 바로 영향을 미치게 되는 단점**도 있다.
- 저장소를 한개만 두는 것의 단점은 만약 **데이터가 소실되었을때 복구가 어려움**

Git
- 개발자가 **자신만의 commit history를 가질 수 있고**, 개발자와 서버의 저장소는 **독립적으로 관리**가 가능.
- commit한 내용에 **실수가 있더라도 이 바로 서버에 영향을 미치지 않음**.
- 개발자는 commit 하다가 자신이 원하는 순간에 서버에 변경 내역(commit history)을 보낼 수 있음. 
- 서버의 통합 관리자는 관리자가 원하는 순간에 각 개발자의 commit history를 가져올 수 있음.

<div style="height: 50px;"></div>

## SVN과 Git 차이점
1. git은 로컬 저장소가 있으므로 **네트워크에 접근할 필요가 없기 때문에 빠름**.
2. svn은 **commit 하는 순간 저장소를 공유하는 모든 개발자들이 보게** 된다. <br>git은 내 로컬 저장소에서 마음껏 개발하고 정리하여 **필요할 때 원격 저장소로 올림**.
3. git의 경우 원격 저장소 서버가 잠시 끊기더라도 **버전 컨트롤이 가능** <br>svn은 서버가 끊기는 순간 **버전 컨트롤도 같이 끊김**.
4. 원격 저장소가 사라지면 svn은 **복구 불가**,<br>git은 로컬 저장소에 사본을 들고 있다면 **복구 가능**.
5. svn은 **저장소가 하나**,<br>git은 로컬 저장소/원격 저장소로 **저장소를 분산**해서 관리  

<img style="margin-left: 30px; width: 500px;" src="/img/posts/형상관리/git_svn.jpg">
<div style="height: 50px;"></div>

## 포스트 작성 시 참고한 링크
- [형상관리란 - 1](https://kurukurucoding.tistory.com/68)
- [형상관리란 - 2](https://sujinnaljin.medium.com/software-engineering-%ED%98%95%EC%83%81-%EA%B4%80%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-932d14f6f341)
- [SVN, GIT 차이점](https://s-yeonjuu.tistory.com/53)


