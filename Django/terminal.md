# 터미널 사용법

## Git 연동

1. Git Bash 실행

2. git config --global user.name "이름"

3. git config --global user.email "이메일"

4. git config --list (정상 등록되었는지 확인)

<br>

## ◈ 터미널 :  CLI를 GUI환경에서 사용 가능하도록 해줌

<br>

> CLI : Command Line Interface
>
>
> GUI : Graphic User Interface

<br>

## ◈ 특수한 디렉터리

> * Home : ~
>
>   → 터미널 구동 시 처음 위치하는 디렉터리
> * Working directory : .
>
>   → 작업중인 현재 위치
> * Root directory : /
>
>   → 모든 디렉터리의 시작점
> * Home : ~
>
>   → 터미널 구동 시 처음 위치하는 디렉터리

<br>

### ● 상위 디렉터리 : ..

### ● 절대 경로 : 루트(/)에서부터 시작

### ● 상대 경로 : 현재(.)에서부터 시작

<br>

## ◈ 터미널 명령어

> 명령어 구조 : 명령어 [옵션] [인자..]
>
> 옵션 : "-"로 시작해서 영문 대소문자로 구성, 명령어의 기능을 구체화
>
> 인자 : 명령어의 수행시 대상이 될 파일이나 디렉터리
>
> ▣ 명령어
>
> * pwd (Print Working Directory) : 현재 위치를 알려준다
> * man (manual) : 명령어 설명서
>>
>>     → man[알고싶은 명령어]
>
> * li (list) : 디렉터리의 목록을 보여준다
>>
>> 옵션 a : 숨김파일까지 보여줌
>>
>> 옵션 l : 상세하게 보여줌
>>
>> 옵션 F : 파일인지 디렉터리인지 알려줌
>
> * cd (Change Directory) : 현재 위치를 이동해줌
>>
>>     → cd [이동하고 싶은 위치]
>
> * clear : 터미널 청소기
>
> * history : 지금까지 입력한 명령어를 순차적으로 다 보여줌 (화살표로 이전에 사용한 명령어 입력 가능)