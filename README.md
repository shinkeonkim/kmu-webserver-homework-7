# Django의 django.contrib.auth 를 활용한 로그인 / 로그아웃
기능 실습

## 제출 내용

제출자: 20191564 김신건
Github (1점) : https://github.com/shinkeonkim/kmu-webserver-homework-7

### a. 로그인화면 캡쳐 (2점)

<img width="284" alt="image" src="https://github.com/user-attachments/assets/1f5e122d-070c-4a38-a054-09eb69297d48" />


### b. login 후 캡쳐 (2점)

<img width="316" alt="image" src="https://github.com/user-attachments/assets/dbde67fe-0b51-469c-a12c-53d2bcbb8365" />


### c. 로그아웃 후 화면 캡쳐 (2점)

<img width="339" alt="image" src="https://github.com/user-attachments/assets/c02b5c14-ee7f-497a-a167-80720945e0ab" />


### d. 코드 완성 (13점)
#### d.1. accounts/views.py (8점)

항목별 내용
1. get('username')
2. get('password')
3. username=username
4. password=password
5. login(request, user)
6. redirect('home')
7. 'accounts/login.html'
8. logout(request)

전체 코드

```python
from django.shortcuts import render, redirect
from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.decorators import login_required
from django.contrib import messages

def login_view(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        user = authenticate(request, username=username, password=password)

        if user is not None:
            login(request, user)
            return redirect('home')
        else:
            messages.error(request, '아이디 또는 비밀번호가 올바르지 않습니다.')
    return render(request, 'accounts/login.html')

@login_required
def home_view(request):
    return render(request, 'accounts/home.html', {'user': request.user})


def logout_view(request):
    logout(request)
    return redirect('login')
```

#### d.2. accounts/urls.py (3점)

항목별 내용
1. views.login_view
2. views.logout_view
3. views.home_view

전체코드
```python
from django.urls import path
from . import views

urlpatterns = [
    path('login/', views.login_view, name='login'),
    path('logout/', views.logout_view, name='logout'),
    path('home/', views.home_view, name='home'),
]
```

#### d.3. templates/accounts/login.html (1점)

항목별 내용
1. {% csrf_token %}

전체 코드
```html
<h2>로그인</h2>
<form method="post">
    {% csrf_token %}
    <input type="text" name="username" placeholder="아이디" required><br>
    <input type="password" name="password" placeholder="비밀번호" required><br>
    <button type="submit">로그인</button>
</form>

{% for message in messages %}
    <p style="color:red;">{{ message }}</p>
{% endfor %}
```

#### d.4. templates/accounts/home.html (1점)

항목별 내용
1. {% url 'logout' %}

2. 전체 코드
```html
<h2>환영합니다, {{ user.username }}님!</h2>
<p><a href="{% url 'logout' %}"> 로그아웃</a></p>
```

