# SW_Digital_System_and_Experiment

## Linux Shell Script를 이용한 CD Collection

MP3 플레이어의 동작과 유사한 음악 CD를 관리하는 프로그램을 만드는 과제를 수행했다.
회원 관리 기능과 함께 CD 대출/반납 기능을 추가했다.
각 음악은 Title, Type, Composer 세 가지 정보를 가진다.
구체적으로 등록, 삭제, 검색, 목록 출력, ㅅ줭, 대출, 반납, 회원관리의 기능이 있다.

## 소스코드
### 각 정보들을 '문자열'로 저장할 파일들을 import
``` sh
#!/bin/sh

# 정보를 저장하고 사용할 파일들을 import 함

Cd_info="cdinfo.cdb"         # CD 목록파일. title, type, composer 순으로 한 라인씩 저장되있다.
Cd_title="cdinfotitle.cdb"   # CD title만 따로 저장하는 파일.
Temp_info="tempfile.cdb"     # 프로그램 진행중에 사용할 임시파일1.
Temp_info2="tempfile2.cdb"   # 프로그램 진행중에 사용할 임시파일2.
Temp_info3="tempfile3.cdb"   # 프로그램 진행중에 사용할 임시파일3.
LentUser_info="userinfo.cdb" # 렌트 회원 파일. "빌린 회원, 회원이 빌린 CD title, 빌린 시간 정보" 가 저장되있는 파일.
LentCd_info="lentinfo.cdb"   # 렌트된 CD 목록파일. 렌트된 CD 들이 저장되있는 파일.
User_log="login.cdb"         # 회원 목록파일.
```


## 프로그램 구동
### 1. 오프닝 & 메인화면
![1](https://user-images.githubusercontent.com/58457978/100039209-1f256700-2e48-11eb-8228-a81530b6d303.png)

### 2. CD 등록
![2](https://user-images.githubusercontent.com/58457978/100039210-1fbdfd80-2e48-11eb-8339-70f6bfe435ac.png)

### 3. CD 목록 출력
![3](https://user-images.githubusercontent.com/58457978/100039204-1e8cd080-2e48-11eb-96cf-a24f0ed991bf.png)
