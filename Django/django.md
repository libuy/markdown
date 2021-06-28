# Django

* **Django** : Python을 사용하는 프레임워크

<br>

* **가상환경** : Python 환경 구축을 위해 필요한 모듈을 담아 놓는 곳

## 가상환경 만들기

````md
* python3 -m venv 가상환경명
* source 가상환경명/Script/activate
* pip  install django
* django -admin statproject 프로젝트 이름
* python manage.py runserver
````

## 앱 Setting

````md
* python manage.py startapp 앱 이름
* settings.py -> INSTALLED_APPS 끝에 '앱 이름.apps.첫 글자는 대문자로 한 앱 이름Config', 추가
````

----

<br>

* **MTV패턴**
>
> Model : DataBase(DB) → Back End
>
> Template : 담당, 사용자가 보이는 영역, 템플릿 언어 → Front End
>
> View : 데이터를 처리하는 곳 → Back End

<br>

* **Template 언어** : HTML에서 파이썬 변수와 문법을 사용하게 해주는 언어

  * 템플릿 변수 : {{ 변수.속성 }}

  * 템플릿 태그 : {% 태그 %},

    ※ {% for %}, {% if %} 등은 뒤에 {% endfor %}, {% endif %} 로 닫아주어야 함

<br>

* **Django 폴더 역할**

    - admin.py: 장고(django)에서 기본적으로 제공하는 관리화면 설정

    - apps.py: 어플리케이션 메인 파일

    - models.py: 어플리케이션의 모델(models) 파일

    - tests.py: 테스트 파일

    - views.py: 어플리케이션의 뷰(views) 파일

<br>


* Path

````python
path('url', view.views내부함수, name='url명')
````

<br>

## Django 와 데이터베이스

<br>

### **- ORM(Object-Relational Mapping)**

: 파이썬의 객체지향적인 방법으로 데이터베이스의 데이터를 다룰 수 있음

### **- Model**
>
> * models.py (모델명의 첫글자는 대문자) : Model 생성
>
>````python
>class 모델명(models.Model)
>   속성 = models.필드타입(옵션)
>````
>
> * 모델 생성 명령어
>   * python manage.py makemigrations
>
>       : 앱 내의 migration 폴더를 만들어서 models.py의 변경사항 저장
>
>   * python manage.py migrate
>
>       : Migration폴더를 실행시켜 데이터베이스에 적용

<br>

### **- Admin**

: 웹(관리자 페이지)에서 편하게 DB를 관리할 수 있도록 해줌

* 슈퍼계정 생성

> python manage.py createsuperuser

* admin 페이지 이동
>
> 1. python manage.py runserver  (슈퍼계정을 만든 후, 서버를 돌림)
>
> 2. url에 /admin 붙임 (관리자 페이지로 이동)
>
> 3. 만든 아이디로 로그인

* admin에 model 등록하기

> 1. admin.py로 이동
>
> 2. from .models import 모델명
>
> 3. admin.site.register(모델명)

<br>

----

## CRUD

: 웹 페이지 상에서 구현하는 생성(Create), 읽기(Read), 수정(Update), 삭제(Delet)의 약어

<br>

* **Path-Converter** : id값을 통해 다른 페이지를 보여주고, views.py의 매개변수로 넘겨줄 수 있음

* **pk (Primary Key)** : 데이터베이스에서 데이터들을 식별하기 위한 고유한 Key

* **get_object_or_404**
  
  404에러 : Object가 없을 때 나타나는 에러

> ````python
> <view.py> →
> 
> def detail (request, blog_id):
>   blog = get_object_or_404(Bolg, pk = blog_id)
>   return render(request,'detail.html',{'blog' : blog})
>
> <urls.py> →
>
> path('<str:id'>, detail,name="detail")
>````

* **GET & POST : http 요청 방식**

> 1. GET
>
>> * 데이터를 얻기 위한 요청
>> * 데이터가 url에 보임
>
> 2. Post
>
>> * 데이터를 생성하기 위한 요청
>> * 데이터가 url에 안보임
>> * Csrf 공격 방지  <br>  ※ {%csrf_token%} 추가 ※

<br>

### -**C**reate

* new : new.html을 보여줌

* create : 데이터베이스에 저장

<br>

1. views.py에서 new.html 추가

````python
<views.py> →
def new(request):
  return render(request,'new.html')
````

2. templates 폴더에 new.html 화면 만들고, urls.py에 링크 연결

````html
path('new/',new,name="new")
````

3. home 화면에 링크 만들기

````html
<a href="{% url 'new' %}> write </a>
````

4. new.html에 글 작성 공간 만들기

````html
<form action="" method="post">
  {%csrf_token%}
  <p>제목 : <input type="text" name="title"></p>
  ...
  본문 <textarea name="body" id="" ~></textarea>
  ...
</form>
````

5. views.py에 create 추가

````python
def create(request):
  new_blog = Blog()
  new_blog.title = request.POST["title"]
  ...
  new_blog.save()
  return redirect('detail',new_blog.id)
````

6. urls.py에 경로 추가

````python
path('create/',create,name="create")
````

7. new.html에 action 연결

> 빈 action에 "{% url 'create'% }" 추가

<br>

* timezone 함수 : 작성한 시각을 나타냄

> ````python
> <views.py> →
>
> from django.utils import timezone
> ...
> new_blog.클래스 변수 = timezone.now()
> ````

<br>

### -**U**date

※ 수정할 데이터의 id 값을 받아야함

* edit : edit.html 보여줌

* update : 데이터베이스에 적용

<br>

1. views.py에 edit 추가

````python
def edit (request, id):
  edit_blog = Blog.objects.get(id=id)
  return render(request,'edit.html',{'blog':edit_blog})
````

2. urls.py에 edit 경로 추가

````python
path('edit/<str:id>',edit,name="edit")
````

3. detail 페이지에 a 태그 추가

````python
<a herf="{% url 'edit' blog.id %}">수정</a>
````

4. edit.html 추가 후 수정

5. view.py에 update 기능 추가

````python
def update(request, id):
    update_blog = Blog.objects.get(id=id)
    update_blog.클래스 변수 = request.POST["이름"]
    ...
    update_blog.save()
    return redirect('detail',update_blog.id)
````

6. urls.py에 update 경로 추가

````python
path('update/<str:id>',update,name="update")
````

7. edit.html에 form action 추가

````python
<form action="{% url 'update' blog.id %}" method="post">
````


<br>

### -**D**elete

1. views.py에 delete 추가

````python
def delete(request,id):
  delete_blog = Blog.objects.get(id=id)
  delete_blog.delete()
  return redirect('home')
````

2. urls.py에 delete 경로 추가

````python
path('delete/<str:id>',delete,name="delete")
````

3. detail.html에 삭제 버튼 추가

````python
<a herf="{% url 'delete' blog.id %}">삭제</a>
````

----

<br>

## Template 상속

<br>

* base.html 파일에 상속할 코드 작성

* base.html의 body 안에 아래 코드 기입

````html
<div class="container">
  {% block content %}
  {% endblock %}
</div>
````

* 상속받을 파일에 아래 형식으로 코드 기입

````html
{% extends 'base.html' %}
{% block content %}
  .
  .
  .
{% endblock %}
````

* settings.py 의 'DIRS'에 ['templates 경로'] 추가

<br>

----
<br>

## 정적 파일

<br>

### **Static**  : 개발자가 서버를 개발할 때 미리 넣어놓은 정적 파일 (img,js,css)

<br>

1. static 폴더 생성 : App 폴더 내에 static 폴더 만들고, 파일 저장

2. settings.py →

````python
STATICFILES_DIRS = [경로(BASE_DIR,'app의 이름','static')]   # 현재 static 파일들이 있는 경로

STATIC_ROOT = 경로(BASE_DIR,'static')   # static 파일을 모으는 곳
````

3. static 파일 모으기 : terminal에 python manage.py collectstatic 입력

4. static 파일 띄우기 : body 상단에 {% load static %} 입력 후, 삽입 할 위치에 {% static '파일'%} 삽입

<br>

### **media** : 사용자가 업로드 할 수 있는 파일

<br>

※ 이미지 업로드 구현해보기


1. settings.py →

````html
MEDIA_ROOT = 경로(BASE_DIR,'midea')   # 이용자가 업로드한 파일을 모으는 곳

MEDIA_URL = '/media/'
````

2. urls.py →

````python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path(...)
    .
    .
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
````

3. models.py →

````python
class Blog(models.Model):
    .
    .
    image = models.ImageField(upload_to="blog/", blank=True, null=True)
    # upload_to : 업로드할 폴더를 지정
````

4. terminal → pip install pillow (imagefield를 사용할 때 설치 필수)

5. 업로드 구현

````html
<form action="{%url 'create'%}"  method="post" enctype="multipart/form-data">
  ..<input type="file" name="image">..
</form>
````

6. views.py → create 내에 new_blog.image = request.FILES['image']

7. 보여주기 : detail.html →

````html
{% if blog.image %}
..<img scr="{{blog.image.url}} alt="">..
{% endif %}
````

----

<br>

## Form

1. form.py 파일 생성 후

````python
from django import forms
from .models import Blog

class BlogForm(forms.ModelForm):
  class Meta:
    model = Blog
    fields = ['models.py 내에 put_date를 제외한 모든 클래스 변수' ]
````

2. views.py →

````python
from .forms import BlogForm

def new(request):
  form = BlogForm()
  return render(request,'new.html',{'form':form})
````

3. new.html에 form태그 구현

4. views.py →

````python
def create(request):
  form = BlogForm(request.POST,request.FILES)
  if form.is_valid():
    new_blog = form.save(commit=False)
    new_blog.pub_date = timezone.now()
    new_blog.save()
    return redirect('detal',new_blog.id)
  return redirect('home')
````

----

<br>

## User 확장과 인증

* auth(authentication)모듈
>
> 1. 클라이언트가 회원가입 요청
> 2. 서버에서 유저테이블에 회원정보 저장
> 3. 클라이언트가 서버에 로그인 요청
> 4. 유저테이블에 정보 유무 확인
> 5. 응답(로그인)

<br>

----

<br>

## Paginator

<br>

: 블로그 객체를 잘라서 보내주는 기능

<br>

### **Queryset Method**

<br>

* .objects.order_by('-pub_date') : 최신글부터 보여줌

* .objects.filer(writer=author) : 조건에 만족하는 값을 반환

* .objects.exlude(writer=author) : 조건에 만족하지 않는 값을 반환
