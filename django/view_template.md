## View, Template 생성기초



### app의 view 추가하기

1. app폴더의 views.py에 함수 추가

   ```python
   # 필요 라이브러리 추가
   from django.shortcuts import HttpResponse
   
   # 어떤 요청이 들어왔을 때, Hello World!를 반환하는 역할
   def index(request): 
       return HttpResponse("Hello World!")
   ```

2. 프로젝트 폴더의 urls.py 로 가서 주소 추가, 해당 앱 함수 import 해주기

   ```python
   # index 함수 불러오기
   from homepage.views import index
   
   urlpatterns = [
   	# 추가해준 주소. 이전까지는 기본 주소에 대한 설정이 따로 없었음(디폴트값으로 존재)
       path('',index), # 127.0.0.1/ 에서 index 함수를 실행.
       
       path("admin/", admin.site.urls), # 127.0.0.1/admin/
   ]
   ```

3. (앱을 처음 만들었다면) 프로젝트 폴더의 settings.py로 가서 INSTALLED _APPS 리스트에 만든 앱 추가

   ```python
   INSTALLED_APPS = [
       "django.contrib.admin",
       "django.contrib.auth",
       "django.contrib.contenttypes",
       "django.contrib.sessions",
       "django.contrib.messages",
       "django.contrib.staticfiles",
       "homepage",     # homepage 앱 추가
   ]
   ```





### Template으로 보여줄 화면 구성



#### render(request, '.html', {사용할 인자들})

- 요청을 받아서, '.html' 파일을 반환하고, 딕셔너리 형태의 인자를 받는다

1. 앱폴더에 template (이름 바꾸면 안됨) 폴더 생성

2. view의 함수가 반환할 html폴더 생성 (예시. index.html)

   ```html
   <!DOCTYPE html>
   <html>
       <head>
           <title>Python django example</title>
       </head>
   
       <body>
           <h1>Title</h1>
           <p>bla bal bal</p>
       </body>
   </html>
   ```

   

3. 프로젝트 폴더의 settings.py의 TEMPLATES의 DIRS에 template경로 추가

   - ```python
     import os
     .
     .
     TEMPLATES = [
         {
             "BACKEND": "django.template.backends.django.DjangoTemplates",
             "DIRS": [
                 os.path.join(BASE_DIR,'homepage','template'), #추가된 부분
                 ],
             "APP_DIRS": True,
             "OPTIONS": {
                 "context_processors": [
                     "django.template.context_processors.debug",
                     "django.template.context_processors.request",
                     "django.contrib.auth.context_processors.auth",
                     "django.contrib.messages.context_processors.messages",
                 ],
             },
         },
     ]
     ```

   - BASE_DIR : 현재 프로젝트가 생성된 폴더의 경로

   - os.path.join() : 들어온 문자열 인자들을 경로로 합쳐서 반환해준다

4. 앱폴더의 view로 돌아가 `return render(request,<template에 있는 파일이름.html>)` 으로 template 파일을 반환

   ```python
   from django.shortcuts import render,HttpResponse
   
   # Create your views here.
   def index(request): # 어떤 요청이 들어왔을 때, Hello World!를 반환하는 역할
       ## 이전 반환
       # return HttpResponse("<h1>Hello World!</h1>")
       
       ## template를 이용한 반환
       return render(request, 'index.html')
   ```

   

##### 인자를 받아서 사용하기

1. 인자를 딕셔너리 형태로 넘긴다

   ```python
   from django.shortcuts import render,HttpResponse
   
   # Create your views here.
   def index(request):
   	number = 121
       name = 'Son'
       return render(request, 'index.html',{'my_num':number,'my_name':name} )
   ```

   

2. render 인자에 해당되는 템플릿으로 가서 다음과 같이 사용 가능 (이전의 index.html)

   ```html
   <!DOCTYPE html>
   <html>
       <head>
           <title>Python django example</title>
       </head>
   
       <body>
           <h1>Title</h1>
           <p>bla bal bal</p>
           <p>{{ my_num }}</p>
           <p>{{ my_name }}</p>
           <p>{{ my_name|length }}</p>
   
       </body>
   </html>
   ```

3. <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221018120257041.png" alt="image-20221018120257041" style="zoom:50%;" />



###### django의 template언어

- template 문법

  - {{ args }} : 인자를 사용
  - ' | ' : 템플릿 필터
    - {{args | length}} : 인자의 길이 사용
    - {{args | upper}} : 문자열 인자 대문자로 등등..

- template 태그 

  - {% tag... %} ~~~{% endtag... %}

  - `{% for a in b %}` ~~~ `{% endfor %}`

  - `{% if a | 조건  %}` ~~~ `{% endif %}`

    - divisibleby : " num " : num으로 나누어 떨어지는지 반환

  - 

    - ```html
      ## my_list의 홀수만 나타내는 html파일
      
      # my_list는 넘겨받은 인자가 될 것
      # my_list = [1,2,3,4,5]
      
      {% for a in my_list %}
      	{% if not a|divisibleby:"2" %}
      	    <p> {{a}} </p>
      	{% endif %}
      {% endfor %}
      ```



