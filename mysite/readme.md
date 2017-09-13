## 1 模型
python manage.py makemigrations polls
python manage.py migrate polls
python manage.py sqlmigrate polls 0001
## 2 Playing with the API
python manage.py shell
```
import os;os.environ.setdefault("DJANGO_SETTINGS_MODULE", mysite.settings)
import django;django.setup()
from django.utils import timezone
Question(question_text="What's new?", pub_date=timezone.now()).save()
Question.objects.all()
Question.objects.filter(id=1)
Question.objects.get(id=1)
q = Question.objects.get(pk=1)
q.was_published_recently()

q.choice_set.all()
q.choice_set.create(choice_text='Not much', votes=0)
q.choice_set.create(choice_text='The sky', votes=0)
c = q.choice_set.create(choice_text='Just hacking agein', votes=0)
c.question
q.choice_set.all()
Choice.objects.filter(question__pub_date__year=current_year)
c = q.choice_set.filter(choice_text__startswith='Just hacking')
c.delete()
```
## 3. Django admin
```
python manage.py createsuperuser
Username: admin
Email address: admin@example.com
Password: asdfghjkl;'
Password (again): asdfghjkl;'
python manage.py runserver
```
http://127.0.0.1:8000/admin/
```buildoutcfg
polls/admin.py
from django.contrib import admin
from .models import Question
admin.site.register(Question)
```
## 4 views
```buildoutcfg
1 question index page  
2 question detail page  
3 question results page  
4 vote action page  
more elegant URL patterns
```
## 5 URL
硬编码:  
`<a href="/polls/{{ question.id }}/">`  
修改为：   
`url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),`   
`<a href="{% url 'detail' question.id %}">{{ question.question_text }}</a>`   
命名空间namespace 用于区分app
```buildoutcfg
# polls/urls.py
app_name = 'polls'
# polls/index.html
<a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a>
```  
## 6 templates
```buildoutcfg
{{ choice.votes}} vote{{ choice.votes|pluralize }}
```
## 7 F
```buildoutcfg
# Tintin filed a news story!
reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed += 1
reporter.save()
===>
Reporters.objects.filter(name='Tintin').update(stories_filed=F('stories_filed') + 1)

# 获取时必须重新载入
reporter = Reporters.objects.get(pk=reporter.pk)
# Or, more succinctly:
reporter.refresh_from_db()
# 原理：class F
# F()是代表模型字段的值，也就是说对于一些特殊的字段的操作，我们不需要用python把数据先取到内存中，然后操作，在存储到db中了。
# 可以避免线程并发带来的不同步
```
## 8 tests

1 test models   
2 test views
```buildoutcfg
python manage.py shell
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "mysite.settings")

from django.test.utils import setup_test_environment
setup_test_environment()

from django.test import Client
client = Client()
resp = client.get("/polls/")
resp.status_code
from django.urls import reverse
resp = client.get(reverse('polls:index'))
resp.status_code
resp.content
resp.context['latest_question_list']
```
## 9 static file
```buildoutcfg
position: polls/static/polls/style.css
html:   {% load static %}
        <link rel="stylesheet" type="text/css" href="{% static 'polls/style.css' %}" />
static: 生成静态文件绝对路径
```
```buildoutcfg
背景图定位在 右上角
body {
    background: white url("images/background.jpg") no-repeat right top;
}
```
## 10 admin 定制
## 11 模板定制
python -c "import django; print(django.__path__)"  
## 12 apps reusable



