# SW_Digital_System_and_Experiment

## Linux Shell Script를 이용한 CD Collection

MP3 플레이어의 동작과 유사한 음악 CD를 관리하는 프로그램을 만드는 과제를 수행했다.
회원 관리 기능과 함께 CD 대출/반납 기능을 추가했다.
각 음악은 Title, Type, Composer 세 가지 정보를 가진다.
구체적으로 등록, 삭제, 검색, 목록 출력, ㅅ줭, 대출, 반납, 회원관리의 기능이 있다.

## 소스코드
### 1. 각 정보들을 '문자열'로 저장할 파일들을 import
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

### 2. 다른 함수에서 쓰일 함수들
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

### 3. CD 
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

### 4. CD 
```sh
#<CD 삭제>
Delete()
{
	
	echo "------------ CD Title, CD Type, CD Composer ------------"
if [ ! -s $Cd_info ]                                                      # CD목록이 비어있으면 "파일이 비어있습니다" 출력 후 메인화면으로
then
        echo "파일이 비어있습니다."
        echo "---------------------------------------------------------"
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
else	
	cat $Cd_info                                                      # CD목록에 정보가 있을 경우 전체목록 출력.
	echo "---------------------------------------------------------"
	echo ""
	echo ""
fi
	
        echo "삭제할 CD title을 입력하세요 : "
        read delName                                              # 삭제할 CD title을 입력받아 'delName'변수에 저장 

	grep $delName $Cd_title > $Temp_info                      # CD title만 따로 저장되있는 $Cd_title파일에서 'delName'에 해당되는 CD를 찾아 $Temp_info에 저장 

        if [ ! -s $Temp_info ]; then                              # $Temp_info에 아무것도 없다면 (= 입력받은 title에 해당하는 CD가 없다면)
        	echo ""
        	echo "해당 CD가 존재하지 않습니다. "
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
		echo ""
		echo ""
		sleep 1
main

	else
                grep -v $delName $Cd_info > $Temp_info                # CD목록($Cd_info)에서 'delName'이 포함된 라인만 제외한 후 $Temp_info에 저장. [grep -v 명령어 : 지정한 패턴에 대응되지 않는 라인들을 보여줌.]
                                     
		mv $Temp_info $Cd_info                                # $Temp_info를 $Cd_info에 저장.
	



		grep -v $delName $Cd_title > $Temp_info3              # $Cd_title파일도 같은 방법으로 업데이트.

		mv $Temp_info3 $Cd_title
		
        	echo ""
        	echo "-------------성공적으로 삭제하였습니다.----------------"
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
		echo ""
		echo ""
		sleep 1
	fi

main
}
```
### 5. CD 목록 출력
```sh
#<CD 목록 출력>
Report()
{
        echo "------------- CD Title, CD Type, CD Composer ------------"
        echo "총 CD의 개수 : "
        if [ ! -s $Cd_info ]
then
        echo "파일이 비어있습니다."
        echo "---------------------------------------------------------"
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
	echo ""
	sleep 1
else
        wc -l $Cd_info >$Temp_info                         # CD목록($Cd_info)파일의 라인 수를 세어 $Temp_info에 저장. 라인 수 = 저장된 CD의 개수
        cut -d" " -f1 $Temp_info >$Temp_info2              # cut -d 명령어를 이용해 " "[공백]을 구분자로 한 첫번째 필드(라인 수)만 $Temp_info2에 저장. 이 작업이 없을 시 $Cd_info의 파일인 'cdinfo.cdb'도 출력된다.
        cat $Temp_info2                                    # 저장된 CD개수 출력.
        cat $Cd_info                                       # CD목록 출력.
        echo "--------------------------------------------------------"
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
	echo ""
	sleep 1
fi
main
}
```

### 6. CD 검색
```sh
# <CD 검색>
Find()
{       echo "---------------- 검색 카테고리 -------------------"
        echo "1. Title  2. Type  3. Composer"
	echo ""
	echo "검색 할 카테고리를 선택하세요 : "
        read num                                                    # 검색할 카테고리를 입력받아 num변수에 저장.
	echo ""
        case $num in                                                # case구문으로 num에 따라 해당 동작 수행.




1) # num=1 인 경우
	echo "<Title 검색>"
        echo "검색 할 Title를 입력하세요 : "
        read title                                                   # 검색 할 Title를 입력받아 'title'변수에 저장.
	echo ""
        echo "...................검 색 중......................."
	sleep 1
	echo "<검색 결과>"
        cut -d, -f1 $Cd_info | grep $title >$Temp_info               # CD목록($Cd_info)파일에서 ','[콤마]를 구분자로 첫번째 필드(Title)에서 입력받은 'title'에 해당되는 라인만 $Temp_info에 저장.

if [ ! -s $Temp_info ]                                               # $Temp_info에 아무것도 없으면 (=입력받은 'title'와 일치하는 CD가 없으면)
then
        echo "해당 Title을 찾을 수 없습니다."
	sleep 1
else
        grep $title $Cd_info >$Temp_info2                            # 해당 CD가 있으면 그 CD 정보를 CD목록에서 찾아 출력.
        cat $Temp_info2
	echo "---------------------------------------------------"
	sleep 1
fi;;




2) # num=2 인 경우
	echo "<Type 검색>"
        echo "검색 할 Type를 입력하세요 : "

        read type
	echo ""
        echo "..................검 색 중......................."
	sleep 1
	echo "<검색 결과>"
        cut -d, -f2 $Cd_info | grep $type >$Temp_info                # 두번째 필드(Type)

if [ ! -s $Temp_info ]
then
        echo "해당 Type을 찾을 수 없습니다."
	sleep 1
else
        grep $type $Cd_info >$Temp_info2
        cat $Temp_info2
	echo "---------------------------------------------------"
	sleep 1
fi;;




3) # num=3 인 경우
	echo "<Composer 검색>"
        echo "검색 할 Composer를 입력하세요 : "
        read composer
	echo ""
        echo ".................검 색 중...................."
	sleep 1
	echo "<검색 결과>"
        cut -d, -f3 $Cd_info | grep $composer >$Temp_info            # 세번째 필드(Composer)

if [ ! -s $Temp_info ]
then
        echo "해당 Composer를 찾을 수 없습니다."
	sleep 1
else
        grep $composer $Cd_info >$Temp_info2
        cat $Temp_info2
	echo "---------------------------------------------------"
	sleep 1
fi;;
esac
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
	echo ""
main
}
```

### 7. CD 대출
```sh
#<CD 대출>
Lent()
{
	if [ ! -s $Cd_info ]                                                      # CD목록이 비어있으면 "파일이 비어있습니다" 출력 후 메인화면으로
then
        echo "-------------------------CD 목록---------------------------"
	echo "------------ CD Title, CD Type, CD Composer ------------"
        echo "파일이 비어있습니다."
        echo "---------------------------------------------------------"
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
else	

        echo "-------------------------CD 목록---------------------------"
	echo "------------ CD Title, CD Type, CD Composer ------------"
        cat $Cd_info
        echo "----------------------------------------------------------"
        sleep 1
        echo ""
        echo ""
        echo "회원 이름을 입력하세요 : "
        read user                                    # 입력받은 회원 이름을 'user'변수에 저장.

        grep $user $User_log > $Temp_info            # 현재 저장된 회원 명부인 $User_log에서 입력받은 'user'회원만 찾아 $Temp_info에 저장.
        if [ ! -s $Temp_info ]; then                 # 회원 명부에 입력받은 'user'가 없다면 "가입 된 회원이 아닙니다. "
        	echo ""
	        echo "가입 된 회원이 아닙니다. "
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

        else
	        echo ""
	        echo "빌릴 CD Title을 입력 하세요 : "
	        read cd                               # 빌릴 CD title을 입력받아 'cd'변수에 저장.
	        echo ""
	        grep $cd $Cd_info >> $LentCd_info     # 빌린 CD를 렌트된 CD 목록에 저장. CD목록($Cd_info)에서 'cd'에 해당하는 라인을 렌트 CD 목록파일($LentCd_info) 뒤에 추가.
	        grep -v $cd $Cd_info > $Temp_info     # 빌린 CD를 CD목록에서 제거. 
	        mv $Temp_info $Cd_info
	        Insert_user $user, $cd, $(date)       # 렌트 정보를 추가하는 함수 Insert_user 함수 호출.
		echo "---------- 성공적으로 대출하였습니다 ----------"
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
        fi
fi
}
```

### 8. CD 반납
```sh
#<CD 반납>
Return()
{
        echo "-------------------- 대출 목록 -------------------------"
        echo "----------대출 한 회원, CD Title, 빌린 시간 -----------"
        cat $LentUser_info
        echo "----------------------------------------------------"

        echo "반납하실 CD Title을 입력 하세요 : "

        read cd                                                     # 반납할 CD title 입력받아 'cd'변수에 저장.
        grep $cd $LentCd_info >> $Cd_info                           # 렌트된 CD 목록파일에서 반납할 CD 찾아서 CD목록($Cd_info)의 뒤에 추가. 
        grep -v $cd $LentCd_info > $Temp_info                    
        mv $Temp_info $LentCd_info                                  # 렌트된 CD 목록파일($LentCd_info) 업데이트.
        grep -v $cd $LentUser_info > $Temp_info2
        mv $Temp_info2 $LentUser_info                               # 렌트 회원 파일($LentUser_info) 업데이트.
        echo "---------------성공적으로 반납 하였습니다----------------"
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

### 9. CD 수정
```sh
#<CD 수정>
Revise()
{
        echo "<CD 정보를 수정하는 곳입니다.>"
	echo ""
	echo "-------------------------CD 목록---------------------------"
	echo "------------ CD Title, CD Type, CD Composer ------------"
if [ ! -s $Cd_info ]                                                      # CD목록이 비어있으면 "파일이 비어있습니다" 출력 후 메인화면으로
then
        echo "파일이 비어있습니다."
        echo "---------------------------------------------------------"
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
else	
	cat $Cd_info                                                       # CD목록에 정보가 있을 경우 전체목록 출력.
	echo "---------------------------------------------------------"
	echo ""
	echo ""
fi
        echo "수정할 CD Title을 입력하세요 : "
        echo ""
    
        read delName                              # 수정할 CD Title 을 delName변수에 저장.
        grep -v $delName $Cd_info > $Temp_info2   # 해당 CD 삭제.
        mv $Temp_info2 $Cd_info                   
        echo "새로운 Title를 입력하세요 : "
        read newTitle
        echo "새로운 Type를 입력하세요 : "
        read newType
        echo "새로운 Composer를 입력하세요 : "
        read newComposer
        Insert_cd $newTitle, $newType, $newComposer   # CD 목록에 수정한 정보 저장.
	echo " --------성공적으로 수정하였습니다 !---------"
	sleep 1
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

main
}
```

### 10. 회원 가입
```sh
#<회원가입>
Add_user()
{
        echo "등록할 회원 이름을 입력하세요 : "
        echo ""
        read nick
        Insert_newbie $nick                                     # 회원목록에 회원을 등록하는 Insert_newbie 함수 호출.
        echo ".............정 상 적 으 로 가 입 되 었 습 니 다.................."
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
Manage_user
}
```

### 11. 회원 찾기
```sh
#<회원 찾기>
Find_user()
{
	echo "---------------- 회원 찾기 ------------------"
	echo "찾을 회원 이름을 입력하세요 : "
	read user
	grep $user $User_log > $Temp_info
	echo ""
	echo "... 검색 중 ..."
	sleep 1
        if [ ! -s $Temp_info ]; then
        echo ""
	echo ""
	echo ""
	echo ""
	echo "-------------- <검색 결과> ----------------"
        echo "그런 회원이 없습니다."
	echo "-------------------------------------------"
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
	
	else	
	echo ""
	echo ""
	echo ""
	echo ""
	echo "-------------- <검색 결과> ----------------"
	cat $Temp_info
	echo "-------------------------------------------"
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
	fi
	sleep 1

Manage_user
}
```

### 12. 회원 삭제
```sh
#<회원 삭제>
Delete_user()
{
	echo "--------------- 회원 삭제 ------------------"
	echo "탈퇴할 회원의 이름을 입력하세요 : "
	read user                              # 탈퇴할 회원 이름 입력받아 'user'변수에 저장.
	grep $user $User_log > $Temp_info      # 회원 목록($User_log)에서 'user'를 찾아 $Temp_info에 저장.
	sleep 1
	if [ ! -s $Temp_info ]; then
     	        echo ""
	        echo ""
		echo ""
		echo ""
		echo ""
   	     echo "가입 된 회원이 아닙니다. "
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
		echo ""
		echo ""
		sleep 1
	Manage_user

	else
		grep -v $user $User_log > $Temp_info   # 회원목록에서 'user'만 삭제.
	
		mv $Temp_info $User_log
		echo ""
                echo ""
		echo ""
		echo ""
		echo ""
		echo "성공적으로 탈퇴 하였습니다 ! "
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
		echo ""
		echo ""
		sleep 1
	fi
	Manage_user
}
```

### 13. 회원 명부 출력
```sh
#<회원 명부>
Report_user(){
	echo "--------- 회원 명부 ---------"
	cat $User_log
	echo "-----------------------------"
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
	echo ""
	echo ""
	sleep 1
Manage_user

}
```

### 14. 회원 관리
```sh
#<회원 관리>
Manage_user()
{

	echo " -----------회원을 관리하는 곳입니다 ! ---------"
	echo " 1. 회원 등록 "
	echo " 2. 회원 탈퇴 "
	echo " 3. 회원 찾기 "
	echo " 4. 회원 명부 "
	echo " 5. 돌아 가기 "
	echo ""
        echo "실행할 기능의 숫자를 입력하세요 ! : "
read numSelect
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
	echo ""
	echo ""

case $numSelect in
1)
        Add_user;;
2)
        Delete_user;;
3)
        Find_user;;
4)
        Report_user;;
5)
	main
esac



main	
}
```

### 15. 오프닝
```sh
#<오프닝>
init()
{
        echo ""
        echo ""
	echo "****************************************************************"
	echo "****************************************************************"
	echo "****************************************************************"
	echo "****************************************************************"
        echo "*********************                     **********************"
        echo "*********************    CD Management    **********************"
        echo "*********************                     **********************"
	echo "****************************************************************"
	echo "****************************************************************"
	echo "****************************************************************"
	echo "****************************************************************"
        echo ""
	echo ""
        echo ""
        echo ""
sleep 2
main          # 오프닝 화면 보여준 후 메인 화면 출력.
} 
```

### 16. 메인 화면 & 프로그램 시작
```sh
#<메인 화면>
main()
{       echo ""
        echo ""
        echo ""
        echo ""
	
        echo "# # # # # # # #  실행 가능한 기능 목록 입니다 ! # # # # # # # # "
	echo "#                                                             #"
        echo "#   1. CD 등 록                                               #"                                               
        echo "#   2. CD 삭 제                                               #"
        echo "#   3. CD 검 색                                               #"
        echo "#   4. CD 전체목록 보기                                       #"
        echo "#   5. CD 수 정                                               #"
        echo "#   6. CD 대 출                                               #"
        echo "#   7. CD 반 납                                               #"
        echo "#   8. 회원 관리                                              #"
	echo "#   9. 프로그램 종료                                          #"
	echo "#                                                             #"
	echo "# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #"
	echo ""
        echo "실행할 기능의 숫자를 입력하세요 ! : "
read numSelect
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
	echo ""
	echo ""
	

case $numSelect in
1)
        Add;;
2)
        Delete;;
3)
        Find;;
4)
        Report;;
5)
        Revise;;
6)
        Lent;;
7)
        Return;;
8)
  	Manage_user;;
9)
	echo "Bye Bye"
        exit 0
esac
}
```

```sh
# 프로그램의 시작!!
init
```




## 프로그램 구동
### 1. 오프닝 & 메인화면
![1](https://user-images.githubusercontent.com/58457978/100039209-1f256700-2e48-11eb-8228-a81530b6d303.png)

### 2. CD 등록
![2](https://user-images.githubusercontent.com/58457978/100039210-1fbdfd80-2e48-11eb-8339-70f6bfe435ac.png)

### 3. CD 목록 출력
![3](https://user-images.githubusercontent.com/58457978/100039204-1e8cd080-2e48-11eb-96cf-a24f0ed991bf.png)
