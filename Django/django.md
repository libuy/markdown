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

----

<br><br>

# 배포

<br>

- 환경 변수

    > 시스템에 저장되어 있는 변수
    >
    > 비밀키 등 유출되면 안되는 정보 또는 환경에 차이를 둘 때 사용(테스트/프로덕션 구별 등)
    >
    > os.environ에서 dict형식으로 불러올 수 있음
    >
    > os.environ.get('변수명','기본값')으로 사용

<br>

- requirments

    > 파이썬(장고) 앱을 실행하기 위해 우선 설치되어야 하는 패키지
    > 
    > 보통 requirments.txt 파일에 저장
    >
    > pip freeze 명령어 : 설치된 모든 패키지 보여줌
    >
    > " > "는 프로그램의 출력을 파일에 저장한다는 뜻

<br>

- IAM (Identity and Access Management)

    > IAM에서 계정을 만든 후 해당 계정 로그인 정보 (엑세스, 시크릿 키)를 이용하여 AWS의 API 활용
    >
    > 보안을 위해 권한을 보수적으로 잡음

<br>

- S3 (Simple Storage Service)

    > AWS에서 제공하는 구글드라이브 정도로 생각할 수 있음
    > 
    > 용량 지정 없이 사용한 만큼만 과금되므로 용량 예측 필요 X
    >
    > 여러 서버 동시 접속 가능(부하 분산 유리)

    ````
    # settings.py

    SECRET_KEY = os.environ.get('SECRET_KEY','이전에 있던 긴거')

    DEBUG = True

    DEFAULT_FILE_STORAGE = 'storages.beackends.s3boto3.S3Boto3Storage'

    AWS_ACCESS_KEY_ID= ''
    AWS_SECRET_ACESS_KEY = ''
    AWS_STROAGE_BUCKET_NAME = ''
    AWS_S3_SIGNATURE_VERSION = ''
    AWS_S3_REGION_NAME =''
    AWS_S3_CUSTOM_DOMAIN = ''
    ````

----

<br>

## Heroku 배포

<br>

1. Heroku 회원가입

    > https://www.heroku.com
    >

<br>

2. Heroku CLI 설치

    > https://devcenter.heroku.com/articles/heroku-cli
    >

<br>

3. 환경 변수 적용

    > - Debug는 아래 값 사용
    >   > DEBUG = (os.environ.get('DEBUG'. 'TRUE') != 'False')
    >

<br>

4. .gitignore 파일 적용

    > - gitignore.io 에서 Django 선택 후 '생성' 클릭
    > - 페이지에 나온 텍스트를 모두 복사후 .gitignore 파일로 저장
    >

<br>

5. Heroku 용 파일 작성

    > - Procfile 이라는 파일을 만들어 아래 내용 작성
    >   > web : guniocorn 프로젝트명.wsgi --log-file-
    > - runtime.txt 파일에 아래 내용 작성
    >   > python-3.9.1
    >

<br>

6. 필요한 Dependency 설치

    > pip install gunicorn whitenoise dj-database-url psycopg2-binary
    >

<br>

7. settings.py 수정

    >
    > 1. Whitenoise 설치
    >
    >   > - MIDDLEWARE 에서 제일 SecurityMiddleware 바로 아래 내용 추가
    >   >   > 'whitenoise.middleware.WhiteNoiseMiddleware',
    >
    > 2. ALLOWED_HOSTS 수정
    >   > - ALLOWED_HOSTS = [] 를 ALLOWED_HOSTS = ['*'] 로 수정
    >
    > 3. DB 관련 코드 수정
    >   > - settings.py 제일 밑에 아래 내용 추가
    >   >   > import dj_database_url <br>
    >   >   > db_from_env = dj_database_url.config(conn_max_age=500) <br>
    >   >   > DATABASES['default'].update(db_from_env)
    >
----

1. requirements.txt 생성

    > pip freez > requirements.txt

2. git에 수정된 파일들 추가

    > git add -A <Br>
    > git commit -m "add files for deplopying to heroku"

3. Heroku 관련 명령어 실행
        >
        > 1. heroku login
        > 2. heroku create
        > 3. git push heroku main
        > 4. heroku run python manage.py migrate
        > 5. heroku run python manage.py createsuperuser
        > 6. heroku open

4. Heroku 에서 환경 변수 설정
        >
        > 1. https://dashboard.heroku.com 에서 앱 선택
        > 2. Settings
        > 3. Config Vars > Reveal Config Vars
        > 4. KEY VALUE 값에 입력후 저장

<br>

----

<br>

## AWS EC2 (Ubuntu) 배포

<br>

1. EC2 인스턴스 만들기

    1. http://console.aws.amazon.com/

    2. 우측 상단 지역이 Seoul 인지 확인

    3. EC2 검색

    4. Instances > Launch Instances

    5. Ubuntu Server 20.04 LTS (64-bit x86)

    6. t2.micro (Free tier eligible)

    7. Storage 설정 (Free Tier 은 30GB까지)

    8. Security Group > Add Rule
        > 1. HTTP
        > 2. HTTPS
        > 3. Custom -> TCP -> 8000 ->0.0.0.0/0, ::/0

    9. Review and Launch > Launch
        > 1. Create a new key pair
        > 2. 이름은 원하는 이름 설정
        > 3. Download Key Pair
        > 4. Launch Instance

<br>

2. Elastic IP 받기

    1. Network & Security > Elastic IPs

    2. Allocate Elastic IP address
    3. Allocate
    4. Associate Elastic IP address
    5. Instance > 아까 생성된 인스턴스 선택
    6. Associate

<br>

3. SSH 연결하기

    > ssh ubuntu@아까_받은_Elastic_IP -i 다운받은_PEM_FILE

<br>

4.  필요한 패키지 설치

    1. sudo apt update && sudo apt -y upgrade

    2. sudo apt install -y python3 python3-pip python3-dev python3-venv build-essential libpq-dev vim git
    3. sudo reboot    *#업그레이드 도중 일부 시스템 파일이 변경되므로 재부팅 추천*

<br>

5. 패키지 설치하는 동안 settings.py 수정

    1. 환경 변수 적용
    
      > 1. Debug 는 아래 값 사용
      >    > DEBUG = (os.environ.get('DEBUG', 'True') != 'False')
      > 2. ALLOWED_HOSTS = [] 를 ALLOWED_HOSTS = [' * '] 로 수정
    
    2. pip freeze > requirements.txt
    
    3. git add -A
    4. git commit -m "edit for deployment"

<br>

6. 우리가 매번 하던것들

     1. python3 -m venv venv
    
     2. git clone 내_GitHub_레포지토리 django-app
     3. cd django-app
     4. source ../venv/ **bin** /activate   *# Windows 처럼 **Scripts** 가 아닌 **bin** 임에 주의*
     5. pip install -r requirements.txt
     6. python manage.py runserver 0.0.0.0:8000
     7. http://내_Elastic_IP:8000 로 접속해서 Django 화면이 표시되는지 확인
     8. deactivate # 가상환경 밖으로 탈출

<br>

7. PostgreSQL 설치

     1. sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
    
     2. wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
     3. sudo apt update
     4. sudo apt -y install postgresql
     5. sudo systemctl enable --now postgresql@13-main  *# 시스템 시작시 자동 실행*
     6. sudo -u postgres createdb django  *# django 는 디비명 - 원하는 이름 입*
     7. sudo -u postgres psql django *# 디비 접속*
     8. create user django with password 'p@ssword!';  *# 원하는 username과 password 입력*
     9. alter role django set client_encoding to 'utf-8';  *# 인코딩 수정 (한글깨짐 방지)*
     10. alter role django set timezone to 'Asia/Seoul';  *#시간 수정*
     11. grant all privileges on database django to django;  *# django 데이터베이스의 모든 권한을 django 유저에게 부여*

<br>

8. settings.py 에 DB 설정

    > 1. 내 컴퓨터에서 VSCode 에서 settings.py 열기
    >
    > 2. DATABASES = 부분을 아래와 같이 수정
    >
    > ````python
    > DATABASES = {
    >    'default': {
    >        'ENGINE': 'django.db.backends.postgresql_psycopg2',
    >        'NAME': os.environ.get('DB_NAME'),
    >        'USER': os.environ.get('DB_USER'),
    >        'PASSWORD': os.environ.get('DB_PASSWORD'),
    >        'HOST': os.environ.get('DB_HOST'),
    >        'PORT': '',
    >    }
    > }
    > ````
    >
    >  1. git add -A
    >  2. git commit -m "edit database"
    >  3. git push

----

<br>

1. 업데이트된 설정 서버에 가져오기 (서버에서 실행)

    1. cd /home/ubuntu/django-app *# git 레포 클론한 위치로 가기*

    2. git pull
    3. source ../venv/bin/activate
    4. pip install psycopg2 # PostgreSQL 접속에 필요한 패키지 설치

<br>

2. 환경 변수 설정 (임시 / 테스트용)

    1. export DB_NAME=django

    2. export DB_USER=django
    3. export DB_PASSWORD='p@ssword!
    4. export DB_HOST=localhost

<br>

3. 잘 작동하는지 테스트

    1. python manage.py makemigrations

    2. python manage.py migrate
    3. python manage.py runserver 0.0.0.0:8000
    4. http://내_Elastic_IP:8000 로 접속해서 Django 화면이 표시되는지 확인 (사진 등 static files는 표시 X)

<br>

4. gunicorn 으로 Django App 실행

    1. pip install gunicorn (반드시 venv 안에서 실행)

    2. deactivate *# 일단 가상환경 탈출*
    3. ../venv/bin/gunicorn 프로젝트명.wsgi -b 0.0.0.0
    4. http://내_Elastic_IP:8000 로 접속해서 Django 화면이 표시되는지 확인 (사진 등 static files는 표시 안되는게 정상)

<br>

5. systemd 에 gunicorn 등록

    - vi 는 에디터 (윈도우에서 메모장 정도의 느낌)

    - 파일을 연 후 i 키 또는 Insert 키 누르면 내용 입력 가능
    - 수정한 사항을 저장하고 싶으면 Esc -> :w -> Enter
    - 종료하고 싶으면 Esc -> :q -> Enter
    - 합하면 Esc -> :wq -> Enter 로 저장 & 종료
    - 저장하지 않고 강제로 종료하려면 Esc -> :q! -> Enter (!는 강제) 


  - sudo vi /etc/systemd/system/gunicorn.service


  ````
  [Unit]
  Description=gunicorn daemon
  Requires=gunicorn.socket
  After=network.target
  
  [Service]
  Type=notify
  RuntimeDirectory=gunicorn
  WorkingDirectory=/home/ubuntu/django-app
  ExecStart=/home/ubuntu/venv/bin/gunicorn 프로젝트명.wsgi
  ExecReload=/bin/kill -s HUP $MAINPID
  KillMode=mixed
  TimeoutStopSec=5
  PrivateTmp=true
  EnvironmentFile=/etc/gunicorn/env.conf
  
  [Install]
  WantedBy=multi-user.target
  ````

  - sudo vi /etc/systemd/system/gunicorn.socket

  ````
  [Unit]
  Description=gunicorn socket
  
  [Socket]
  ListenStream=/run/gunicorn.sock
  SocketUser=www-data
  
  [Install]
  WantedBy=sockets.target
  ````

  - sudo mkdir /etc/gunicorn
  - sudo vi /etc/gunicorn/env.conf

  ````
  DB_NAME=django
  DB_USER=django
  DB_PASSWORD='p@ssword!'
  DB_HOST=localhost
  #기타 환경변수 여기에 입력
  ````

  1. 생성한 서비스들 실행
      > 1. sudo systemctl daemon-reload
      >
      > 2. sudo systemctl enable --now gunicorn.socket
      > 3. sudo systemctl enable --now gunicorn

  2. 테스트 

      > 1. curl --unix-socket /run/gunicorn.sock http
      >
      > - 실행시 HTML 코드가 나오면 성공

  1. Nginx 설치

      > - Nginx는 웹서버로 외부 세상 <-> Gunicorn 을 연결하는 역할을 함
      > 1. sudo apt -y install nginx
      > 2. sudo vi /etc/nginx/sites-available/django_app

   

   ````
   server {
          listen 80;
          server_name 내_Elastic_IP_주소;
   
          location = /favicon.ico { access_log off; log_not_found off; }
   
          location /static/ {
                  root /home/ubuntu/django-app;
          }
   
          location / {
                  include proxy_params;
                  proxy_pass http://unix:/run/gunicorn.sock;
          }
   }
   ````

  1. sudo ln -s /etc/nginx/sites-available/django_app /etc/nginx/sites-enabled
  
  2. sudo nginx -t
  3. sudo systemctl restart nginx
  4. http://내_Elastic_IP 접속시 Django 화면이 보이면 성공!
  5. source ../venv/bin/activate
  6. python manage.py collectstatic # 이미지나 CSS등 Static 파일들

----

<br>

## Docker

<br>

- 세팅

1. gitpod.io
2. 설정에서 feature previw, Theia 체크
3. 터미널 
    
    > sudo docker-up
    >
    >새 터미널
    >
    >docker run -it centos bash <br>
    >cat /etc/os-release

<br>

----

## Docker로 배포하기(1. 이미지 생성)

<br>

1. 준비 사항

    1. Gitpod.io 계정

    2. Gitpod 설정 수정

        > - setting → feature preview > enable feature preview 체크
        > - Default IDE 선택

    3. Gitpod 인스턴스 생성
        > 1. 본인의 GitHub 레포지토리 이동
        > 2. 레포지토리 주소 앞에 gitpod.io/# 붙임
    4. DockerHub 계정 생성

2. requirements 설ㅊ

    > pip install -r requirements.txt

3. Whitenoise 설치

    > 1. pip install whitenoise
    >
    > 2. MIDDLEWARE 에서 SecurityMiddleware 바로 아래 내용 추가
    >     > 'whitenoise.middleware.WhiteNoiseMiddleware'.

4. Gunicorn 설ㅊ

    > pip install gunicorn

5. Dockerfile 생성

    ````
    FROM python:3.8
    ENV PYTHONUNBUFFERED=1
    RUN mkdir /app
    WORKDIR /app
    COPY . /app
    RUN apt-get update \
      && apt-get install -y \
      python3 python3-pip python3-dev python3-venv build-essential libpq-dev \
      && rm -rf /var/lib/apt/lists/*
    RUN pip install -r requirements.txt
    RUN chmod +x /app/run.sh
    EXPOSE 8000

    ENTRYPOINT ["/app/run.sh"]
    ````

    - run.sh 파일 생성

    ````
    #!/bin/bash

    python manage.py migrate

    python manage.py collectstatic

    gunicorn lionproject.wsgi -b 0.0.0.0:8000
    ````

    1. pip freeze > requirements.txt *# requirements 파일 생성*

    2. sudo docer-up   *# 이 창 닫으면 안 됌*
    3. Ctrl + Shift + ` 로 새 터미널 열기
    4. docker build -t DockerHubld/django-app . *# 도커 이미지 생성*
    5. docker run -it -p 8000:8000 DockerHubld/django-app *# 생성된 도커 이미지 실행*
    6. docker login
    7. docker push DockerHubld/django-app *# 생성된 이미지 업로드*
    8. https://hub.docker.com/r/DockerHubld/django-app 에서 업로드 확인

----

<br>

## Docker로 배포하기(2. 서버 배포)

<br>

1. 준비사항

    1. EC2 인스턴스 (AWS EC2 전반부 참고)
    2. Docker Hub 에 등록된 이미지

<br>

2. 서버 세팅

    1. Docker 설치

        > curl -fsSL https://get.docker.com | bash
    2. Docker Compose 설치
        >1. sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose$(uname -s )-$(uname-m)" -o/usr/local/bin/docker-compose *# docker-compose 바이너리 다운로드*
        >2. sudo chmod +x /usr/local/bin/docker-compose *# docker-compose 바이너리 실행 권한 부여*

    3. 앱 설치 폴더 지정
    
        > 1. sudo mkdir /opt/django-app
        > 2. cd /opt/django-app
    4. docker-compose.yml
        > vi docker-compose.yml

  ```` yml
   version: "3.9"
   
   services:
    db:
      image: postgres
      environment:
        - POSTGRES_DB=${DB_NAME}
        - POSTGRES_USER=${DB_USER}
        - POSTGRES_PASSWORD=${DB_PASSWORD}
    web:
      image: DockerHubId/django-app
      environment:
        - DB_NAME=${DB_NAME}
        - DB_USER=${DB_USER}
        - DB_PASSWORD=${DB_PASSWORD}
        - DB_HOST=db
        - DEBUG=${DEBUG}
      ports:
        - "8000:8000"
      depends_on:
        - db
  ````

  - 위 내용을 그대로 터미널에 복.붙 할 경우 띄어쓰기 문제가 발생할 수 있으므로 pastebin.com 등의 서비스에 업로드 후 wget 사용

  - sudo wget https://pastebin.com/raw/pasteld -0 docker-compose.yml

  - .env 파일 생성

      ````
      DB_NAME=django
      DB_USER=django
      DB_PASSWORD=django
      DEBUG=False
      ````


1. DB 최초 실행 (db 생성 , 사용자 등록 등 진행)

    > sudo docker-compose up db

2. 잘 작동 되는지 테스트

    > 1. sudo docker-compse up
    > 2. 내 IP:8000 접속 되는 확인

3. 백그라운드 모드 전환

    > sudo docker-compose up -d

---

<br>

## Docker로 배포하기(3. 이미지 자동 생성하기)

<br>

1. 준비사항

    1. Docker 배포하기가 끝난 레포
    2. Github 계정

2. GitHub의 내 레포로 이동

3. Actions 버튼 선택

- Setup a workflow yourself 선택

- 이름을 docker-publish.yml로 설정 후 아래 내용 작성

````yml
name: Docker Publish

on:
 push:
   branches: [ main ]

 # Allows you to run this workflow manually from the Actions tab
 workflow_dispatch:

jobs:
 build:
   runs-on: ubuntu-latest

   steps:
     - uses: actions/checkout@v2

     - name: Docker Build
       run: docker build -t ${{ secrets.DOCKER_USERNAME }}/my-app .

     - name: Docker Push
       run: |
         docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
         docker push ${{ secrets.DOCKER_USERNAME }}/my-app
````



- Start commit 버튼으로 수정사항 커밋

- Settings > Secrets
- New Repository Secret 생성

- DOCKER_USERNAME , DOCKER_PASSWORD 저장


※ 레포에 새로운 내용을 푸시한 후 Actions 탭으로 가면 GitHub에서 내 앱을 자동으로 빌드하고 푸시하는 것을 볼 수 있음.
