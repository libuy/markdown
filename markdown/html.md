# HTML

<br>

## ◈ **HTML 문서임을 알려주는 태그**

<br>

````html
<!DOCTYPE html>
<html>


</html>
````

````html
<!DOCTYPE html>
<html lang="ko">
        * 한국말로 작성된 문서임을 표시함


</html>
````

- - -  
<br>

## ◈ **head 태그**

→ 화면에 직접 나오지 않고 문서를 설명하는 태그

````html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">   * 인코딩 방식은 utf-8
        <title>제목</title>      * 탭에 표시되는 이름 설정
    </head>
    .
    .
</html>
````

- - -
<br>

## ◈ **body 태그**

→ 직접적으로 화면에 나오는, 문서에서 보이는 태그

````html
<!DOCTYPE html>
<html>
    .
    .
    <body>

        * 본문 내용 작성
         
    </body>
</html>
````

※ !(느낌표) + Tap → 바로 기본세팅 완료  
<br>

> ### **◇ body 안에 쓰이는 태그**  
>
><br>
>
> ● 텍스트와 관련된 태그
>>
>> * 제목 태그 : h1,h2,h3...등의 태그로 제목을 나타내는 태그  (숫자가 커질수록 글자가 작아짐)
>>
>>   ````html
>>   <h1> ~  </h1>, <h2> ~ </h2>...
>>   ````
>>
>><br>
>>
>> * 본문 태그
>>
>>   * P 태그 : 단락을 구분 (아무리 엔터를 쳐도 줄바꿈 불가능)
>>
>>     ````html
>>     <p> 단락 </p>
>>     ````
>>
>>   * br 태그 : 줄바꿈을 해줌 (종료 태그를 쓰지 않는 빈요소)
>>
>>     ````html
>>     ~<br>
>>     ````
>>
>>   * pre 태그 : 입력한 모양 그대로 브라우저에 표시 (들여쓰기에 주의)
>>
>>     ````html
>>     <pre> 단락 </pre>
>>     ````
>>
>>   * strong, em 태그 : 문장 혹은 단어를 strong은 볼드체, em은 이탤릭체로 강조
>>
>>     ````html
>>     <strong> 강조할 문장 or 단어 </strong>
>>     <em>강조할 문장 or 단어</em>
>>     ````
>>
>>   * sub, sup 태그 : 글자의 위치를 일반 글자보다 위, 아래로 바꿈
>>
>>     ````html
>>     <sub> 아래로 내릴 글자 </sub>
>>     <sup> 위로 올릴 글자 </sup>
>>     ````
>>
>>   * ins, del 태그 : 밑줄, 취소선 추가
>>
>>     ````html
>>     <ins> 밑줄을 추가할 문장 </ins>
>>     <del> 취소선을 추가할 문장 </del>
>>     ````
>>
><br>
>
>● 링크 태그
>
>   ````html
>    <a href = "연결할 링크">이름</a>
>   ````
>
>● 멀티미디어와 관련된 태그
>>
>> * 이미지 태그 : 이미지를 삽입하는 태그
>>
>>   ````html
>>   <img src = "이미지 url">
>>   <img src = "이미지 url" alt="사진 설명">
>>   <img src = "이미지 url" height="사이즈" width="사이즈">
>>   ````
>>
>>> *(이미지 파일은 html과 같은 폴더에 있어야함)
><br>
>
>● 테이블 태그
>>
>> * table 태그 : 표 전체를 감싸는 태그
>> * tr 태그 : 표에서 행을 구분하는 태그
>> * th 태그 : 표의 행 내부에 제목 셀을 담는 태그
>> * td 태그 : 표의 행 내부에 데이터 셀을 담는 태그
>>
>>   ````html
>>   <table>
>>     <tr>                  # 1행
>>         <th>1열 제목</th>    # 1행 1열
>>         <th>2열 제목</th>    # 1행 2열
>>         <th>3열 제목</th>    # 1행 3열
>>     </tr>
>>     <tr>                  # 2행
>>        <td>데이터1</td>      # 2행 1열 데이터
>>        <td>데이터2</td>      # 2행 2열 데이터
>>        <td>데이터3</td>      # 2행 3열 데이터
>>     </tr>
>>   ````
>>
>><br>
>>
>> * colspan 태그 : 숫자만큼 셀이 열을 점유
>> * rowspan 태그 : 숫자만큼 셀이 행을 점유
>>
>>   ````html
>>   <table>
>>     <tr>
>>         <th>1열 제목</th>
>>         <th>2열 제목</th> 
>>         <th>3열 제목</th>
>>     </tr>
>>     <tr> 
>>        <td>데이터1</td> 
>>        <td colspan="2">데이터2</td>  # 2행 2열과 3열이 한 칸이 됌
>>     </tr>
>>   ````
>>
><br>
>
>● 리스트
>>
>> * li 태그 : 리스트의 항목들을 나타냄
>>   * value 태그 : 해당하는 리스트 아이템의 번호를 지정
>> * ul 태그 : 순서가 없는 리스트
>>
>>   ````html
>>   <ul>
>>     <li>항목</li>
>>     <li>항목</li>
>>        .
>>        .
>>   </ul>
>>   ````
>>
>> * ol 태그 : 순서가 있는 리스트
>>   * start 태그 : 리스트가 시작하는 숫자를 정함
>>   * type 태그 : 순서를 시작하는 문자를 정함
>>   * reversed 태그 : 순서를 반대로 시작
>>
>>   ````html
>>   <ol start="3">
>>     <li>항목1</li>               # "3.항목1"로 출력
>>     <li>항목2</li>               # "4.항목2"로 출력
>>     <li value="8">항목3</li>     # "8.항목3"으로 출력
>>     <li>항목4</li>               # "9.항목4"로 출력
>>   </ol>
>>   ````
>>
>> ※ 리시트 내에 하위의 항목 생성이 가능함
>
><br>
>
>● 폼 태그
>>
>>
>> * action : 데이터를 보낼 URL을 지정
>> * method : 보내는 방식을 지정
>>   * get 방식(method="get") : 데이터를 URL 끝에 붙여 눈에 보이게 보냄 <br>
→ 데이터 조회 만을 목적으로 할 때 주로 쓰임
>>   * post 방식(method="post") : 데이터를 URL에 적지 않고 내부에 숨겨서 보냄 <br>
→ 서버에 있는 데이터를 쓰거나 수정, 삭제 할 때 주로 사용
>> * input 태그 : 사용자에게 입력을 받기 위해 사용됨(빈태그)
>>   * input type="text" : text 타입으로 입력됨
>>   * input type="password" : ●●●● 형태로 입력됨
>>   * placeholder : 입력란 추가
>>   * label 태그 : 해당하는 라벨을 클릭 시 input태그가 활성화 됨
>>   * textarea 태그 : 한 번에 많은 글을 입력 받을 때 사용
>>
>> <br>
>>
>> -기본적인 폼 태그의 형태
>>
>>   ````html
>>   <form action = "전송받을 대상" mathod = "보내는 방식">
>>          <input type = "타입지정" name = "입력받은 값의 이름" placeholder = "입력란 문구">
>>   </form>
>>   ````
>>
>> -회원가입 화면으로 보는 태그 사용법
>>
>>   ````html
>>   <form action = "전송받을 대상" mathod = "보내는 방식">
>>          <label for="userid">아이디: </label>
>>          <input type="text" name="id" placeholder="아이디를 입력해주세요">
>>          <label for="password">비밀번호: </label>
>>          <input type="password" name="password" placeholder="비밀번호를 입력해주세요">
>>          <div>
>>              <label for="introduce">자기소개: </label>
>>              <textarea name="introduce" id="introduce" cols="행의 글자 수" rows="줄의 수" placeholder="자기소개를 입력하세요"></textarea>
>>          </div>
>>          <button type="summit">제출</button>
>>          <button type="reset">리셋</button>
>>   </form>
>>   ````
>>
><br>
>
>
> ● select 태그 : 선택목록을 만드는 태그
>
>   ````html
>   <select name="옵션 이름">
>     <option value = "옵션값저장">표시되는 각 옵션 이름</option>
>     <option value = "옵션값저장">표시되는 각 옵션 이름</option>
>   </select>
>   ````
>