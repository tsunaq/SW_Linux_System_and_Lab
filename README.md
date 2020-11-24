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

### 다른 함수에서 쓰일 함수들
```sh
# CD목록($Cd_info)파일에 CD정보를 저장하는 함수.
Insert_cd()
{
        echo $* >>$Cd_info                                             # 정규표현식(*)을 이용하여 문자'$'가 0번이상 나타나는 문자열을
								       # 파일리다이렉션(>>)을 이용하여 Cd_info 파일의 뒤에 추가. 즉, "title, type, composer" 가 추가됨.(콤마 ',' 포함)

}


# Insert_cdtitle(), Insert_user(), Insert_newbie() 함수들은 Insert_cd() 함수와 같은 동작을 수행한다.



# CD title만 따로 $Cd_title에 추가하는 함수. 
Insert_cdtitle()
{
	echo $* >>$Cd_title

}




# 렌트 정보 추가하는 함수. "빌린 회원, 회원이 빌린 CD title, 빌린 시간 정보" 를 렌트 정보 파일($LentUser_info) 뒤에 추가한다.
Insert_user()
{
        echo $* >>$LentUser_info
	

}



# 회원 목록 추가 함수.
Insert_newbie()
{
        echo $* >>$User_log
}
```

### 
```sh
#<CD 등록>

Add()
{
	echo "----------------- CD 등록 절차 --------------------"
	echo ""
        echo "등록 할 CD Title를 입력하세요 : "
        read title                                                   # title 이라는 변수에 입력받은 'Title'을 문자열로 저장
        echo "등록 할 CD Type를 입력하세요 : "
        read type                                                    # type 이라는 변수에 입력받은 'Type'을 문자열로 저장
        echo "등록 할 CD Composer를 입력하세요 : "
        read composer                                                # composer 라는 변수에 입력받은 'Composer'를 문자열로 저장




	Insert_cdtitle $title                                        # CD title만 따로 추가하는 Insert_cdtitle 함수 호출하여 $title 전달.
        Insert_cd      $title, $type, $composer                      # CD 목록에 입력받은 CD 정보 추가. Insert_cd 함수 호출하여 $title, $type, $composer 전달.
	echo "----------성공적으로 추가시켰습니다 !------------"
	echo ""
	echo ""
	echo ""
	echo ""
	echo ""
	echo ""
	echo ""
	echo ""
	echo ""
	echo ""
	echo ""
	sleep 1
main

	
}
```

## 프로그램 구동
### 1. 오프닝 & 메인화면
![1](https://user-images.githubusercontent.com/58457978/100039209-1f256700-2e48-11eb-8228-a81530b6d303.png)

### 2. CD 등록
![2](https://user-images.githubusercontent.com/58457978/100039210-1fbdfd80-2e48-11eb-8339-70f6bfe435ac.png)

### 3. CD 목록 출력
![3](https://user-images.githubusercontent.com/58457978/100039204-1e8cd080-2e48-11eb-96cf-a24f0ed991bf.png)
