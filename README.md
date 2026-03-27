# Jinja Template Engine

### 1.1. Jinja Template Engine nima?

**Jinja** - bu Python uchun mo'ljallangan zamonaviy va kuchli **template engine** (shablon dvigateli) bo'lib, HTML sahifalarini dinamik tarzda yaratish uchun ishlatiladi. Jinja 2 versiyasi hozirda eng keng qo'llaniladigan versiya hisoblanadi.

Jinja dastlab 2006-yilda **Armin Ronacher** tomonidan yaratilgan va hozirda Python web-development olamida standart hisoblanadi.

**Oddiy tushuntirish:**
Tasavvur qiling, sizda bir xil ko'rinishdagi, lekin har safar boshqa ma'lumotlar bilan to'ldirilishi kerak bo'lgan HTML sahifa bor. Masalan, har bir foydalanuvchi o'z profilini ko'rganda, sahifa dizayni bir xil, lekin ismi, rasmi va ma'lumotlari boshqa. Jinja aynan shu ishni osonlashtiradi - siz bitta "shablon" (template) yaratasiz va kerakli joylariga o'zgaruvchilar qo'yasiz.

### 1.2. Qayerlarda ishlatiladi?

**Flask** web-framework Jinja'ni o'zining standart template engine sifatida ishlatadi. **Django** esa o'zining Django Template Language (DTL) sistemasiga ega, lekin DTL va Jinja juda o'xshash sintaksisga ega.

**Jinja ishlatilishi:**
- Flask loyihalarida (default)
- Ansible (konfiguratsiya boshqaruvi)
- Salt (infrastruktura boshqaruvi)
- Static site generatorlar
- Email templatelar yaratish
- PDF generatsiya qilish

### 1.3. Template Engine nima va nega kerak?

**Template Engine** - bu ma'lumotlarni (data) va ko'rinishni (presentation/view) ajratib turuvchi texnologiya.

**Muammo:** Tasavvur qiling, siz Python kodida HTML yozmoqchisiz:

```python
# ❌ Yomon yondashuv
def create_page(users):
    html = "<html><body><ul>"
    for user in users:
        html += f"<li>{user['name']} - {user['email']}</li>"
    html += "</ul></body></html>"
    return html
```

**Muammolar:**
- Kod o'qish qiyin
- HTML va Python aralash
- Katta loyihalarda boshqarish qiyin
- Dizaynerlar bilan ishlash murakkab

**✅ Yechim - Template Engine:**

```python
# Python (Backend)
users = [
    {'name': 'Ali', 'email': 'ali@example.com'},
    {'name': 'Vali', 'email': 'vali@example.com'}
]
return render_template('users.html', users=users)
```

```html
<!-- users.html (Frontend) -->
<html>
<body>
    <ul>
    {% for user in users %}
        <li>{{ user.name }} - {{ user.email }}</li>
    {% endfor %}
    </ul>
</body>
</html>
```

**Afzalliklari:**
- ✅ Kod toza va tushunarli
- ✅ Frontend va Backend ajratilgan
- ✅ Dizaynerlar HTML bilan ishlashi mumkin
- ✅ Qayta ishlatish oson

### 1.4. Server-side Rendering (SSR) nima?

**Server-side Rendering** - bu HTML sahifaning to'liq serverda tayyorlanib, tayyor holda foydalanuvchi brauzeriga yuborilishi.

**Jarayon:**

```
1. Foydalanuvchi: http://example.com/users ga so'rov yuboradi
   ↓
2. Server: Ma'lumotlar bazasidan foydalanuvchilar ro'yxatini oladi
   ↓
3. Server: Jinja template'ga ma'lumotlarni yuboradi
   ↓
4. Jinja: Template'ni ma'lumotlar bilan to'ldiradi (render qiladi)
   ↓
5. Server: Tayyor HTML'ni foydalanuvchiga yuboradi
   ↓
6. Brauzer: Tayyor HTML'ni ko'rsatadi
```

**Client-side Rendering (React, Vue) dan farqi:**

| Server-side (Jinja) | Client-side (React) |
|-------------------|-------------------|
| HTML serverda yaratiladi | HTML brauzerda yaratiladi |
| Tezroq yuklash | Dastlabki yuklash sekin |
| SEO uchun yaxshi | SEO uchun qo'shimcha sozlash kerak |
| Har safar sahifa qayta yuklanadi | Single Page Application (SPA) |

---

## Jinja Arxitekturasi va Asosiy Tushunchalar

### 2.1. Template (Shablon)

**Template** - bu HTML (yoki boshqa matn formatdagi) fayl bo'lib, ichida Jinja sintaksisi mavjud.

**Oddiy tushuntirish:**
Template - bu "bo'sh joylar" bor tayyor shakl. Masalan, pasport arizasida sizning ismingiz, tugilgan kuningiz uchun bo'sh joylar bor. Siz shu bo'sh joylarga ma'lumotlaringizni yozasiz.

**Template fayl misoli:**
```html
<!-- greeting.html -->
<h1>Salom, {{ name }}!</h1>
<p>Siz {{ age }} yoshdasiz.</p>
```

### 2.2. Context (Kontekst)

**Context** - bu template'ga yuboriladigan ma'lumotlar (data) lug'ati (dictionary).

```python
# Python kodida
context = {
    'name': 'Sardor',
    'age': 25,
    'city': 'Toshkent'
}
render_template('greeting.html', **context)
```

**Tushuntirish:**
Context - bu template'dagi bo'sh joylarni to'ldirish uchun kerak bo'lgan ma'lumotlar. Python lug'at (dictionary) formatida bo'ladi.

### 2.3. Variable Rendering (O'zgaruvchilarni ko'rsatish)

**Variable Rendering** - template ichidagi o'zgaruvchilarni HTML'ga aylantirish jarayoni.

**Sintaksis:** `{{ variable_name }}`

```html
<!-- Template -->
<p>Foydalanuvchi: {{ username }}</p>
<p>Email: {{ user_email }}</p>
```

```python
# Python
render_template('page.html', username='Ali', user_email='ali@mail.uz')
```

**Natija:**
```html
<p>Foydalanuvchi: Ali</p>
<p>Email: ali@mail.uz</p>
```

### 2.4. Expressionlar (Ifodalar)

Jinja'da Python'ga o'xshash ifodalarni yozish mumkin:

```html
{{ 5 + 5 }}  <!-- 10 -->
{{ "Salom " + "Dunyo" }}  <!-- Salom Dunyo -->
{{ user.name }}  <!-- Obyekt attributiga kirish -->
{{ users[0] }}  <!-- List elementiga kirish -->
{{ price * 1.2 }}  <!-- Matematik amallar -->
```

### 2.5. Control Structures (Boshqaruv strukturalari)

**Control Structures** - shart operatorlari va sikllarni bildiradi.

**Sintaksis:** `{% ... %}`

```html
{% if user_logged_in %}
    <p>Xush kelibsiz!</p>
{% else %}
    <p>Iltimos, tizimga kiring</p>
{% endif %}

{% for item in items %}
    <li>{{ item }}</li>
{% endfor %}
```

### 2.6. Template Inheritance (Shablon meros olish)

**Template Inheritance** - bir template'dan ikkinchisiga strukturani meros olish.

**Tushuntirish:**
Tasavvur qiling, sizda 100 ta sahifa bor va barchasi bir xil header, footer va sidebar'ga ega. Agar header'ni o'zgartirish kerak bo'lsa, 100 ta faylni o'zgartirish kerak bo'ladi. Template inheritance bu muammoni hal qiladi - bitta "base" template yaratiladi va qolgan sahifalar undan meros oladi.

**Struktura:**
```
base.html (Asosiy shablon)
   ↓
index.html (Meros oladi)
about.html (Meros oladi)
contact.html (Meros oladi)
```

### 2.7. Filters (Filtrlar)

**Filters** - o'zgaruvchini chiqarishdan oldin o'zgartirish uchun ishlatiladi.

**Sintaksis:** `{{ variable | filter_name }}`

```html
{{ name | upper }}  <!-- ISMI KATTA HARF BILAN -->
{{ text | length }}  <!-- Matn uzunligi -->
{{ price | round(2) }}  <!-- 2 ta kasr bilan yaxlitlash -->
```

**Tushuntirish:**
Filter - bu o'zgaruvchini "pipedan o'tkazish" demakdir. Ma'lumot birinchi tarafdan kiradi, filter uni o'zgartiradi va ikkinchi tarafdan chiqaradi.

### 2.8. Macros (Makroslar)

**Macro** - qayta ishlatish mumkin bo'lgan kod bloklari (Python'dagi funksiyaga o'xshash).

```html
{% macro input_field(name, type='text') %}
    <input type="{{ type }}" name="{{ name }}" class="form-control">
{% endmacro %}

<!-- Ishlatish -->
{{ input_field('username') }}
{{ input_field('password', 'password') }}
```

**Tushuntirish:**
Macro - bu template ichidagi "funktsiya". Bir marta yozib, keyin ko'p marta ishlatish mumkin.

### 2.9. Blocks (Bloklar)

**Block** - template inheritance'da o'zgartirish mumkin bo'lgan qismlar.

```html
<!-- base.html -->
<html>
<head>
    <title>{% block title %}Default Title{% endblock %}</title>
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>
```

```html
<!-- index.html -->
{% extends "base.html" %}

{% block title %}Bosh sahifa{% endblock %}

{% block content %}
    <h1>Salom!</h1>
{% endblock %}
```

### 2.10. Autoescape (Avtomatik himoya)

**Autoescape** - HTML maxsus belgilarni avtomatik kodlash (XSS hujumlaridan himoya).

```html
<!-- user_input = "<script>alert('XSS')</script>" -->

{% autoescape true %}
    {{ user_input }}  
    <!-- Natija: &lt;script&gt;alert('XSS')&lt;/script&gt; -->
{% endautoescape %}
```

**Tushuntirish:**
Autoescape foydalanuvchi kiritgan HTML kodlarni avtomatik "xavfsiz" formatga o'giradi, shunda zararli kod bajarilmaydi.

### 2.11. Commentlar (Izohlar)

**Comment** - template ichidagi izohlar (HTML'ga chiqmaydi).

```html
{# Bu Jinja izoh - HTML'da ko'rinmaydi #}

{# 
   Ko'p qatorli
   izoh ham
   yozish mumkin
#}

<!-- Bu HTML izohi - HTML kodda ko'rinadi -->
```

### 2.12. Whitespace Control (Bo'sh joylarni boshqarish)

**Whitespace Control** - ortiqcha bo'sh qatorlar va bo'shliqlarni olib tashlash.

```html
{%- if true -%}
    Matn
{%- endif -%}
```

**Tushuntirish:**
- `-` belgisi oldidagi yoki keyingi bo'sh joylarni olib tashlaydi
- `{%-` - oldingi bo'shliqni olib tashlaydi
- `-%}` - keyingi bo'shliqni olib tashlaydi

---

## Jinja O'rnatish va Ishlatish

### 3.1. Flask bilan minimal Jinja loyihasi

**Windows uchun:**
```bash
# 1. Virtual environment yaratish
python -m venv venv

# 2. Virtual environment'ni faollashtirish
venv\Scripts\activate

# 3. Flask o'rnatish
pip install Flask

# 4. Loyihani ishga tushurish
python app.py
```

**Linux/MacOS uchun:**
```bash
# 1. Virtual environment yaratish
python3 -m venv venv

# 2. Virtual environment'ni faollashtirish
source venv/bin/activate

# 3. Flask o'rnatish
pip install Flask

# 4. Loyihani ishga tushurish
python app.py
```

### 3.2. Loyiha strukturasi

```
my_project/
│
├── venv/                 # Virtual environment
├── templates/            # Jinja template'lar bu yerda
│   ├── base.html
│   ├── index.html
│   └── about.html
├── static/              # CSS, JS, rasmlar
│   ├── css/
│   ├── js/
│   └── images/
└── app.py               # Asosiy Flask aplikatsiya
```

### 3.3. `templates/` papkasining vazifasi

**templates/** papkasi - Flask avtomatik ravishda shu papkadan Jinja template'larni qidiradi.

**Muhim qoidalar:**
- ✅ Papka nomi **templates** bo'lishi kerak (boshqa nom bo'lsa ishlamaydi)
- ✅ Papka `app.py` bilan bir direktoriyada bo'lishi kerak
- ✅ Barcha HTML fayllar shu papkada
- ✅ Ichki papkalar yaratish mumkin: `templates/admin/dashboard.html`

### 3.4. `render_template()` funksiyasi

**render_template()** - Jinja template'ni render qilib, HTML qaytaradi.

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    # Oddiy render
    return render_template('index.html')

@app.route('/user/<name>')
def user(name):
    # Ma'lumot yuborish
    return render_template('user.html', username=name)

@app.route('/dashboard')
def dashboard():
    # Ko'p ma'lumot yuborish
    data = {
        'title': 'Dashboard',
        'users': ['Ali', 'Vali', 'Guli'],
        'count': 150
    }
    return render_template('dashboard.html', **data)
```

**Parametrlar:**
- Birinchi parametr: template fayl nomi
- Qolgan parametrlar: template'ga yuborilgan o'zgaruvchilar

### 3.5. Minimal loyiha misoli

**app.py:**
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html', 
                         name='Sardor', 
                         age=25)

if __name__ == '__main__':
    app.run(debug=True)
```

**templates/index.html:**
```html
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Mening Sahifam</title>
</head>
<body>
    <h1>Salom, {{ name }}!</h1>
    <p>Siz {{ age }} yoshdasiz.</p>
</body>
</html>
```

**Natija:** Brauzernida `http://127.0.0.1:5000` ochilganda "Salom, Sardor! Siz 25 yoshdasiz." ko'rinadi.

---

## Jinja Sintaksisi - Batafsil

### 4.1. O'zgaruvchilar (Variables)

**a) Oddiy o'zgaruvchi:**
```python
# app.py
@app.route('/hello')
def hello():
    return render_template('hello.html', message='Xayr!')
```

```html
<!-- hello.html -->
<p>{{ message }}</p>  <!-- Natija: Xayr! -->
```

**b) Dictionary (lug'at) dan ma'lumot:**
```python
@app.route('/user')
def user():
    user_data = {
        'name': 'Ali',
        'age': 30,
        'city': 'Toshkent'
    }
    return render_template('user.html', user=user_data)
```

```html
<!-- user.html -->
<p>Ism: {{ user.name }}</p>
<p>Yosh: {{ user.age }}</p>
<p>Shahar: {{ user['city'] }}</p>  <!-- Ikki yo'l ham ishlaydi -->
```

**c) List (ro'yxat) dan element:**
```python
@app.route('/fruits')
def fruits():
    fruits = ['Olma', 'Nok', 'Uzum']
    return render_template('fruits.html', fruits=fruits)
```

```html
<!-- fruits.html -->
<p>Birinchi meva: {{ fruits[0] }}</p>  <!-- Olma -->
<p>Ikkinchi meva: {{ fruits[1] }}</p>  <!-- Nok -->
```

**d) Class obyektidan ma'lumot:**
```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
    
    def get_full_info(self):
        return f"{self.name} ({self.email})"

@app.route('/profile')
def profile():
    user = User('Vali', 'vali@mail.uz')
    return render_template('profile.html', user=user)
```

```html
<!-- profile.html -->
<p>Ism: {{ user.name }}</p>
<p>Email: {{ user.email }}</p>
<p>To'liq: {{ user.get_full_info() }}</p>
```

### 4.2. Jinja Filters - Batafsil

#### a) `|upper` - Katta harflarga o'girish

```html
{{ "salom dunyo" | upper }}  
<!-- Natija: SALOM DUNYO -->

{{ name | upper }}  
<!-- Agar name = "Ali" bo'lsa, natija: ALI -->
```

**Amaliy misol:**
```python
@app.route('/title')
def title():
    return render_template('title.html', title='python dasturlash')
```

```html
<h1>{{ title | upper }}</h1>
<!-- Natija: <h1>PYTHON DASTURLASH</h1> -->
```

#### b) `|lower` - Kichik harflarga o'girish

```html
{{ "PYTHON" | lower }}  
<!-- Natija: python -->

{{ company_name | lower }}
```

#### c) `|title` - Har bir so'zni katta harf bilan boshlash

```html
{{ "python dasturlash tili" | title }}
<!-- Natija: Python Dasturlash Tili -->
```

**Amaliy misol:**
```python
@app.route('/book')
def book():
    return render_template('book.html', 
                         book_name='o`tkan kunlar')
```

```html
<h2>{{ book_name | title }}</h2>
<!-- Natija: O`tkan Kunlar -->
```

#### d) `|length` - Uzunlikni aniqlash

```html
{{ "Salom" | length }}  
<!-- Natija: 5 -->

{{ [1, 2, 3, 4, 5] | length }}  
<!-- Natija: 5 -->
```

**Amaliy misol:**
```python
@app.route('/products')
def products():
    products = ['Laptop', 'Telefon', 'Planshet', 'Soat']
    return render_template('products.html', products=products)
```

```html
<p>Jami mahsulotlar: {{ products | length }} ta</p>
<!-- Natija: Jami mahsulotlar: 4 ta -->
```

#### e) `|replace` - Matnni almashtirish

```html
{{ "Salom dunyo" | replace("dunyo", "Jahon") }}
<!-- Natija: Salom Jahon -->
```

**Amaliy misol:**
```python
@app.route('/privacy')
def privacy():
    text = "Bu mahsulot ACME kompaniyasi tomonidan ishlab chiqarilgan."
    return render_template('privacy.html', 
                         text=text, 
                         company='ACME', 
                         new_company='UZTECH')
```

```html
<p>{{ text | replace(company, new_company) }}</p>
<!-- Natija: Bu mahsulot UZTECH kompaniyasi tomonidan ishlab chiqarilgan. -->
```

#### f) `|safe` - HTML kodni xavfsiz deb belgilash

**⚠️ Ehtiyot: Faqat ishonchli ma'lumotlarda ishlating!**

```python
@app.route('/content')
def content():
    html_content = "<strong>Bu qalin matn</strong>"
    return render_template('content.html', content=html_content)
```

```html
<!-- safe filtersiz -->
{{ content }}
<!-- Natija: &lt;strong&gt;Bu qalin matn&lt;/strong&gt; -->

<!-- safe filter bilan -->
{{ content | safe }}
<!-- Natija: <strong>Bu qalin matn</strong> (qalin ko'rinadi) -->
```

#### g) `|join` - Ro'yxatni birlashtrish

```html
{{ ['Ali', 'Vali', 'Guli'] | join(', ') }}
<!-- Natija: Ali, Vali, Guli -->

{{ ['Python', 'Java', 'JavaScript'] | join(' va ') }}
<!-- Natija: Python va Java va JavaScript -->
```

**Amaliy misol:**
```python
@app.route('/team')
def team():
    team_members = ['Sardor', 'Aziza', 'Bobur', 'Dilnoza']
    return render_template('team.html', team=team_members)
```

```html
<p>Jamoa a'zolari: {{ team | join(', ') }}</p>
<!-- Natija: Jamoa a'zolari: Sardor, Aziza, Bobur, Dilnoza -->
```

#### h) `|default` - Default qiymat berish

```html
{{ username | default('Mehmon') }}
<!-- Agar username mavjud bo'lmasa, "Mehmon" ko'rsatiladi -->

{{ user.phone | default('Telefon ko\'rsatilmagan') }}
```

**Amaliy misol:**
```python
@app.route('/greeting')
def greeting():
    # name parametri bo'lmasligi mumkin
    name = request.args.get('name')
    return render_template('greeting.html', name=name)
```

```html
<h1>Salom, {{ name | default('Mehmon') }}!</h1>
<!-- URL: /greeting → Salom, Mehmon! -->
<!-- URL: /greeting?name=Ali → Salom, Ali! -->
```

#### i) `|trim` - Bo'sh joylarni kesish

```html
{{ "   Python   " | trim }}
<!-- Natija: "Python" -->
```

#### j) `|capitalize` - Birinchi harfni katta qilish

```html
{{ "python" | capitalize }}
<!-- Natija: Python -->

{{ "uzbekistan" | capitalize }}
<!-- Natija: Uzbekistan -->
```

#### k) `|sort` - Tartiblash

```html
{{ [3, 1, 4, 1, 5, 9] | sort }}
<!-- Natija: [1, 1, 3, 4, 5, 9] -->

{{ ['Vali', 'Ali', 'Guli'] | sort }}
<!-- Natija: ['Ali', 'Guli', 'Vali'] -->
```

**Amaliy misol:**
```python
@app.route('/students')
def students():
    students = ['Zafar', 'Aziza', 'Bobur', 'Dilnoza']
    return render_template('students.html', students=students)
```

```html
<h3>Alifbo bo'yicha:</h3>
<ul>
{% for student in students | sort %}
    <li>{{ student }}</li>
{% endfor %}
</ul>
<!-- Natija: Aziza, Bobur, Dilnoza, Zafar -->
```

### 4.3. Control Structures

#### a) `if / elif / else` - Shartlar

**Misol 1: Oddiy shart**
```python
@app.route('/check-age')
def check_age():
    return render_template('check_age.html', age=20)
```

```html
{% if age >= 18 %}
    <p>Siz katta yoshdasisiz</p>
{% else %}
    <p>Siz voyaga yetmagansiz</p>
{% endif %}
```

**Misol 2: Ko'p shartlar**
```python
@app.route('/grade')
def grade():
    return render_template('grade.html', ball=85)
```

```html
{% if ball >= 90 %}
    <p class="excellent">A'lo - {{ ball }} ball</p>
{% elif ball >= 80 %}
    <p class="good">Yaxshi - {{ ball }} ball</p>
{% elif ball >= 70 %}
    <p class="average">O'rtacha - {{ ball }} ball</p>
{% else %}
    <p class="poor">Qoniqarsiz - {{ ball }} ball</p>
{% endif %}
```

**Misol 3: Login tekshirish**
```python
@app.route('/dashboard')
def dashboard():
    user = session.get('user')  # Foydalanuvchi tizimga kirganmi?
    return render_template('dashboard.html', user=user)
```

```html
{% if user %}
    <h1>Xush kelibsiz, {{ user.name }}!</h1>
    <a href="/logout">Chiqish</a>
{% else %}
    <h1>Iltimos, tizimga kiring</h1>
    <a href="/login">Kirish</a>
{% endif %}
```

#### b) `for` loop - Sikl

**Misol 1: Oddiy ro'yxat**
```python
@app.route('/fruits')
def fruits():
    fruits = ['Olma', 'Nok', 'Uzum', 'Shaftoli', 'Banan']
    return render_template('fruits.html', fruits=fruits)
```

```html
<h2>Mevalar ro'yxati:</h2>
<ul>
{% for fruit in fruits %}
    <li>{{ fruit }}</li>
{% endfor %}
</ul>

<!-- Natija:
Mevalar ro'yxati:
• Olma
• Nok
• Uzum
• Shaftoli
• Banan
-->
```

**Misol 2: Dictionary bilan**
```python
@app.route('/students')
def students():
    students = [
        {'name': 'Ali', 'age': 20, 'grade': 'A'},
        {'name': 'Vali', 'age': 21, 'grade': 'B'},
        {'name': 'Guli', 'age': 19, 'grade': 'A'}
    ]
    return render_template('students.html', students=students)
```

```html
<table border="1">
    <thead>
        <tr>
            <th>Ism</th>
            <th>Yosh</th>
            <th>Baho</th>
        </tr>
    </thead>
    <tbody>
    {% for student in students %}
        <tr>
            <td>{{ student.name }}</td>
            <td>{{ student.age }}</td>
            <td>{{ student.grade }}</td>
        </tr>
    {% endfor %}
    </tbody>
</table>
```

**Misol 3: Dictionary kalitlari va qiymatlari bilan**
```python
@app.route('/settings')
def settings():
    settings = {
        'theme': 'dark',
        'language': 'uz',
        'notifications': True,
        'auto_save': False
    }
    return render_template('settings.html', settings=settings)
```

```html
<h3>Sozlamalar:</h3>
<dl>
{% for key, value in settings.items() %}
    <dt>{{ key | title }}:</dt>
    <dd>{{ value }}</dd>
{% endfor %}
</dl>
```

#### c) `loop` obyekti

Jinja har bir `for` sikl ichida maxsus `loop` obyekti yaratadi:

**`loop` obyektining attributlari:**
- `loop.index` - Joriy iteratsiya raqami (1 dan boshlanadi)
- `loop.index0` - Joriy iteratsiya raqami (0 dan boshlanadi)
- `loop.first` - Birinchi iteratsiyami? (True/False)
- `loop.last` - Oxirgi iteratsiyami? (True/False)
- `loop.length` - Jami elementlar soni
- `loop.cycle()` - Qiymatlarni navbatma-navbat qaytarish

**Misol 1: `loop.index`**
```html
<ol>
{% for item in ['Python', 'Java', 'JavaScript', 'C++'] %}
    <li>{{ loop.index }}. {{ item }}</li>
{% endfor %}
</ol>

<!-- Natija:
1. Python
2. Java
3. JavaScript
4. C++
-->
```

**Misol 2: `loop.first` va `loop.last`**
```python
@app.route('/products')
def products():
    products = ['Laptop', 'Telefon', 'Planshet', 'Soat']
    return render_template('products.html', products=products)
```

```html
<ul>
{% for product in products %}
    <li 
        {% if loop.first %}class="first-item"{% endif %}
        {% if loop.last %}class="last-item"{% endif %}
    >
        {{ product }}
        {% if loop.first %} (Eng muhim!){% endif %}
        {% if loop.last %} (Oxirgi element){% endif %}
    </li>
{% endfor %}
</ul>
```

**Misol 3: `loop.cycle()` - Ranglarni almashtirish**
```html
<table>
{% for student in students %}
    <tr style="background-color: {{ loop.cycle('white', 'lightgray') }}">
        <td>{{ loop.index }}</td>
        <td>{{ student.name }}</td>
    </tr>
{% endfor %}
</table>

<!-- Har bir qator navbatma-navbat oq va kulrang bo'ladi -->
```

**Misol 4: `loop.length` - Jami soni**
```html
<h3>Talabalar (Jami: {{ loop.length }})</h3>
<ul>
{% for student in students %}
    <li>
        {{ student.name }} 
        ({{ loop.index }} / {{ loop.length }})
    </li>
{% endfor %}
</ul>
```

#### d) Bo'sh ro'yxat tekshirish - `{% else %}`

```python
@app.route('/tasks')
def tasks():
    tasks = []  # Bo'sh ro'yxat
    return render_template('tasks.html', tasks=tasks)
```

```html
<h2>Vazifalar ro'yxati:</h2>
<ul>
{% for task in tasks %}
    <li>{{ task }}</li>
{% else %}
    <li>Hech qanday vazifa yo'q</li>
{% endfor %}
</ul>

<!-- Agar tasks bo'sh bo'lsa: "Hech qanday vazifa yo'q" ko'rsatiladi -->
```

#### e) `set` operatori - O'zgaruvchi yaratish

```html
{% set total = 100 %}
{% set discount = 10 %}
{% set final_price = total - discount %}

<p>Asosiy narx: {{ total }} so'm</p>
<p>Chegirma: {{ discount }} so'm</p>
<p>Yakuniy narx: {{ final_price }} so'm</p>
```

**Amaliy misol:**
```html
<table>
{% set row_count = 0 %}
{% for user in users %}
    {% set row_count = row_count + 1 %}
    <tr>
        <td>{{ row_count }}</td>
        <td>{{ user.name }}</td>
    </tr>
{% endfor %}
</table>
<p>Jami qatorlar: {{ row_count }}</p>
```

#### f) `break` va `continue` analoglari

⚠️ **Muhim:** Jinja'da `break` va `continue` yo'q, lekin o'xshash natijaga erishish mumkin.

**`break` o'rniga:**
```html
{% for i in range(100) %}
    {% if i > 10 %}
        {# Siklni to'xtatish mumkin emas, lekin shart bilan cheklash mumkin #}
    {% else %}
        <p>{{ i }}</p>
    {% endif %}
{% endfor %}
```

**`continue` o'rniga:**
```html
{% for user in users %}
    {% if user.active %}
        <li>{{ user.name }}</li>
    {# Agar active False bo'lsa, chiqarilmaydi #}
    {% endif %}
{% endfor %}
```

### 4.4. Template Inheritance - Juda batafsil

Template inheritance - bu Jinja'ning eng kuchli xususiyatlaridan biri.

#### a) `base.html` yaratish

**base.html** - barcha sahifalar uchun asosiy shablon:

```html
<!-- templates/base.html -->
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    {# Sahifa title'i - har bir sahifa o'zgartiradi #}
    <title>{% block title %}Default Title{% endblock %}</title>
    
    {# CSS fayllar #}
    <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
    
    {# Qo'shimcha CSS - har bir sahifa o'z CSS qo'shishi mumkin #}
    {% block extra_css %}{% endblock %}
</head>
<body>
    {# Header - barcha sahifalarda bir xil #}
    <header>
        <nav>
            <ul>
                <li><a href="/">Bosh sahifa</a></li>
                <li><a href="/about">Biz haqimizda</a></li>
                <li><a href="/contact">Aloqa</a></li>
            </ul>
        </nav>
    </header>

    {# Main content - har bir sahifa o'zgartiradi #}
    <main>
        {% block content %}
        <p>Default content</p>
        {% endblock %}
    </main>

    {# Footer - barcha sahifalarda bir xil #}
    <footer>
        <p>&copy; 2024 Mening Saytim. Barcha huquqlar himoyalangan.</p>
    </footer>

    {# JavaScript fayllar #}
    <script src="{{ url_for('static', filename='js/main.js') }}"></script>
    
    {# Qo'shimcha JavaScript #}
    {% block extra_js %}{% endblock %}
</body>
</html>
```

**Tushuntirish:**
- `{% block title %}...{% endblock %}` - Bu joyni boshqa sahifalar o'zgartirishi mumkin
- `{% block content %}...{% endblock %}` - Asosiy kontent joyi
- `{% block extra_css %}{% endblock %}` - Bo'sh block, kerak bo'lsa to'ldiriladi

#### b) `extends` bilan foydalanish

**index.html** - Bosh sahifa:

```html
<!-- templates/index.html -->
{% extends "base.html" %}

{% block title %}Bosh sahifa - Mening Saytim{% endblock %}

{% block content %}
    <h1>Xush kelibsiz!</h1>
    <p>Bu bizning asosiy sahifamiz.</p>
    
    <div class="features">
        <div class="feature">
            <h3>Tezlik</h3>
            <p>Saytimiz juda tez ishlaydi</p>
        </div>
        <div class="feature">
            <h3>Xavfsizlik</h3>
            <p>Ma'lumotlaringiz xavfsiz</p>
        </div>
    </div>
{% endblock %}

{% block extra_js %}
    <script>
        console.log('Bosh sahifa yuklandi');
    </script>
{% endblock %}
```

**about.html** - Biz haqimizda sahifasi:

```html
<!-- templates/about.html -->
{% extends "base.html" %}

{% block title %}Biz haqimizda{% endblock %}

{% block extra_css %}
    <style>
        .team { display: grid; grid-template-columns: repeat(3, 1fr); }
    </style>
{% endblock %}

{% block content %}
    <h1>Biz haqimizda</h1>
    <p>Biz 2020-yildan beri faoliyat yuritamiz.</p>
    
    <div class="team">
        <div class="member">
            <h3>Ali</h3>
            <p>Direktor</p>
        </div>
        <div class="member">
            <h3>Vali</h3>
            <p>Dasturchi</p>
        </div>
        <div class="member">
            <h3>Guli</h3>
            <p>Dizayner</p>
        </div>
    </div>
{% endblock %}
```

#### c) Ko'p darajali meros

**Tuzilma:**
```
base.html (Asosiy)
   ↓
dashboard_base.html (Dashboard uchun)
   ↓
admin_users.html (Admin foydalanuvchilar sahifasi)
```

**base.html:**
```html
<!-- base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>
    {% block body %}{% endblock %}
</body>
</html>
```

**dashboard_base.html:**
```html
<!-- dashboard_base.html -->
{% extends "base.html" %}

{% block body %}
    <div class="dashboard">
        <aside class="sidebar">
            {% block sidebar %}
            <ul>
                <li><a href="/dashboard">Dashboard</a></li>
                <li><a href="/profile">Profil</a></li>
                <li><a href="/settings">Sozlamalar</a></li>
            </ul>
            {% endblock %}
        </aside>
        
        <main class="content">
            {% block dashboard_content %}{% endblock %}
        </main>
    </div>
{% endblock %}
```

**admin_users.html:**
```html
<!-- admin_users.html -->
{% extends "dashboard_base.html" %}

{% block title %}Foydalanuvchilar - Admin{% endblock %}

{% block sidebar %}
    {{ super() }}  {# Ota-template sidebar'ini saqlab qolish #}
    <hr>
    <h4>Admin menyusi</h4>
    <ul>
        <li><a href="/admin/users">Foydalanuvchilar</a></li>
        <li><a href="/admin/settings">Admin sozlamalari</a></li>
    </ul>
{% endblock %}

{% block dashboard_content %}
    <h1>Foydalanuvchilar ro'yxati</h1>
    <table>
        {% for user in users %}
        <tr>
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
        </tr>
        {% endfor %}
    </table>
{% endblock %}
```

**Tushuntirish:**
- `{{ super() }}` - Ota-template block kontent ini saqlab qolish

### 4.5. Include - Komponentlar

`{% include %}` - boshqa template'ni joriy template'ga qo'shish.

#### a) Header, Footer, Sidebar misollari

**Struktura:**
```
templates/
├── base.html
├── index.html
├── components/
│   ├── header.html
│   ├── footer.html
│   └── sidebar.html
```

**components/header.html:**
```html
<header>
    <div class="logo">
        <h1>Mening Saytim</h1>
    </div>
    <nav>
        <ul>
            <li><a href="/">Bosh sahifa</a></li>
            <li><a href="/about">Biz haqimizda</a></li>
            <li><a href="/contact">Aloqa</a></li>
            {% if user %}
                <li><a href="/profile">{{ user.name }}</a></li>
                <li><a href="/logout">Chiqish</a></li>
            {% else %}
                <li><a href="/login">Kirish</a></li>
            {% endif %}
        </ul>
    </nav>
</header>
```

**components/footer.html:**
```html
<footer>
    <div class="footer-content">
        <div class="footer-section">
            <h3>Biz haqimizda</h3>
            <p>Eng yaxshi xizmatlar</p>
        </div>
        <div class="footer-section">
            <h3>Aloqa</h3>
            <p>Email: info@example.com</p>
            <p>Tel: +998 90 123 45 67</p>
        </div>
        <div class="footer-section">
            <h3>Ijtimoiy tarmoqlar</h3>
            <a href="#">Facebook</a>
            <a href="#">Instagram</a>
            <a href="#">Telegram</a>
        </div>
    </div>
    <p>&copy; 2024 Barcha huquqlar himoyalangan</p>
</footer>
```

**base.html (include bilan):**
```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>
    {% include 'components/header.html' %}
    
    <main>
        {% block content %}{% endblock %}
    </main>
    
    {% include 'components/footer.html' %}
</body>
</html>
```

#### b) Parametr yuborish bilan Include

**product_card.html:**
```html
<!-- components/product_card.html -->
<div class="product-card">
    <img src="{{ product.image }}" alt="{{ product.name }}">
    <h3>{{ product.name }}</h3>
    <p>{{ product.description }}</p>
    <p class="price">{{ product.price }} so'm</p>
    <button>Sotib olish</button>
</div>
```

**products.html:**
```python
@app.route('/products')
def products():
    products = [
        {'name': 'Laptop', 'price': 5000000, 'image': 'laptop.jpg', 'description': 'Tez va qudratli'},
        {'name': 'Telefon', 'price': 3000000, 'image': 'phone.jpg', 'description': 'Zamonaviy dizayn'}
    ]
    return render_template('products.html', products=products)
```

```html
<!-- products.html -->
{% extends "base.html" %}

{% block content %}
    <h1>Mahsulotlar</h1>
    <div class="products-grid">
    {% for product in products %}
        {% include 'components/product_card.html' %}
    {% endfor %}
    </div>
{% endblock %}
```

#### c) Shartli Include

```html
{% if user.is_admin %}
    {% include 'components/admin_panel.html' %}
{% endif %}

{% if show_sidebar %}
    {% include 'components/sidebar.html' %}
{% endif %}
```

---

## Jinja'da Macro va Functions

### 5.1. Macro nima?

**Macro** - bu template ichidagi "funktsiya". Bir marta yozib, ko'p marta ishlatish mumkin.

**Oddiy tushuntirish:**
Tasavvur qiling, sizda 20 ta forma bor va har birida bir xil `<input>` teglar. Har safar yozish o'rniga, bitta macro yaratib, uni ishlatish mumkin.

### 5.2. Oddiy Macro

**Misol 1: Tugma yaratish**
```html
{# Macro ta'rifi #}
{% macro button(text, type='button', class='btn') %}
    <button type="{{ type }}" class="{{ class }}">
        {{ text }}
    </button>
{% endmacro %}

{# Ishlatish #}
{{ button('Saqlash', 'submit', 'btn btn-primary') }}
{{ button('Bekor qilish') }}
{{ button('O\'chirish', class='btn btn-danger') }}
```

**Natija:**
```html
<button type="submit" class="btn btn-primary">Saqlash</button>
<button type="button" class="btn">Bekor qilish</button>
<button type="button" class="btn btn-danger">O'chirish</button>
```

### 5.3. Parametrlar va Default qiymatlar

```html
{% macro input_field(name, label='', type='text', required=False, placeholder='') %}
    <div class="form-group">
        {% if label %}
            <label for="{{ name }}">{{ label }}</label>
        {% endif %}
        <input 
            type="{{ type }}" 
            name="{{ name }}" 
            id="{{ name }}"
            {% if required %}required{% endif %}
            {% if placeholder %}placeholder="{{ placeholder }}"{% endif %}
            class="form-control"
        >
    </div>
{% endmacro %}

{# Ishlatish #}
{{ input_field('username', label='Foydalanuvchi nomi', required=True) }}
{{ input_field('email', label='Email', type='email', placeholder='email@example.com') }}
{{ input_field('password', label='Parol', type='password', required=True) }}
```

### 5.4. Form yaratishda Macro

**macros/forms.html:**
```html
{# Input field #}
{% macro input(name, label, type='text', value='', required=False) %}
<div class="form-group">
    <label for="{{ name }}">
        {{ label }}
        {% if required %}<span class="required">*</span>{% endif %}
    </label>
    <input 
        type="{{ type }}" 
        name="{{ name }}" 
        id="{{ name }}"
        value="{{ value }}"
        {% if required %}required{% endif %}
        class="form-control"
    >
</div>
{% endmacro %}

{# Textarea #}
{% macro textarea(name, label, rows=4, required=False) %}
<div class="form-group">
    <label for="{{ name }}">
        {{ label }}
        {% if required %}<span class="required">*</span>{% endif %}
    </label>
    <textarea 
        name="{{ name }}" 
        id="{{ name }}"
        rows="{{ rows }}"
        {% if required %}required{% endif %}
        class="form-control"
    ></textarea>
</div>
{% endmacro %}

{# Select dropdown #}
{% macro select(name, label, options, selected='', required=False) %}
<div class="form-group">
    <label for="{{ name }}">
        {{ label }}
        {% if required %}<span class="required">*</span>{% endif %}
    </label>
    <select 
        name="{{ name }}" 
        id="{{ name }}"
        {% if required %}required{% endif %}
        class="form-control"
    >
        <option value="">Tanlang...</option>
        {% for value, text in options %}
            <option value="{{ value }}" {% if value == selected %}selected{% endif %}>
                {{ text }}
            </option>
        {% endfor %}
    </select>
</div>
{% endmacro %}
```

**register.html:**
```html
{% extends "base.html" %}
{% from 'macros/forms.html' import input, textarea, select %}

{% block content %}
<h1>Ro'yxatdan o'tish</h1>
<form method="POST" action="/register">
    {{ input('username', 'Foydalanuvchi nomi', required=True) }}
    {{ input('email', 'Email', type='email', required=True) }}
    {{ input('password', 'Parol', type='password', required=True) }}
    {{ input('password_confirm', 'Parolni tasdiqlang', type='password', required=True) }}
    
    {{ select('country', 'Davlat', [
        ('uz', 'O\'zbekiston'),
        ('kz', 'Qozog\'iston'),
        ('tj', 'Tojikiston')
    ], required=True) }}
    
    {{ textarea('bio', 'O\'zingiz haqingizda', rows=5) }}
    
    <button type="submit" class="btn btn-primary">Ro'yxatdan o'tish</button>
</form>
{% endblock %}
```

### 5.5. Macro'dan Macro chaqirish

```html
{# Asosiy card macro #}
{% macro card(title, content, footer='') %}
<div class="card">
    <div class="card-header">
        <h3>{{ title }}</h3>
    </div>
    <div class="card-body">
        {{ content }}
    </div>
    {% if footer %}
    <div class="card-footer">
        {{ footer }}
    </div>
    {% endif %}
</div>
{% endmacro %}

{# User card - card macro'ni ishlatadi #}
{% macro user_card(user) %}
    {{ card(
        title=user.name,
        content='<p>Email: ' ~ user.email ~ '</p><p>Yosh: ' ~ user.age ~ '</p>',
        footer='<a href="/user/' ~ user.id ~ '">Batafsil</a>'
    ) }}
{% endmacro %}
```

### 5.6. `call` bilan ishlash

`call` - macro ichiga kontent yuborish imkonini beradi.

```html
{% macro dialog(title) %}
<div class="dialog">
    <div class="dialog-title">{{ title }}</div>
    <div class="dialog-content">
        {{ caller() }}  {# Bu yerda call ichidagi kontent ko'rsatiladi #}
    </div>
</div>
{% endmacro %}

{# Ishlatish #}
{% call dialog('Ogohlantirish') %}
    <p>Bu muhim xabar!</p>
    <p>Davom etasizmi?</p>
    <button>Ha</button>
    <button>Yo'q</button>
{% endcall %}
```

---

## Jinja'da Custom Filterlarni Yaratish

### 6.1. Custom Filter nima?

Ba'zan Jinja'ning o'rnatilgan filtrlari yetarli bo'lmaydi. Siz o'zingizning maxsus filterlaringizni yaratishingiz mumkin.

### 6.2. Oddiy Custom Filter

**app.py:**
```python
from flask import Flask, render_template

app = Flask(__name__)

# Custom filter yaratish
@app.template_filter('reverse')
def reverse_filter(text):
    """Matnni teskari o'girish"""
    return text[::-1]

@app.route('/')
def index():
    return render_template('index.html', name='Python')
```

**index.html:**
```html
<p>{{ name }}</p>          <!-- Python -->
<p>{{ name | reverse }}</p>  <!-- nohtyP -->
```

### 6.3. Parametrli Custom Filter

```python
@app.template_filter('repeat')
def repeat_filter(text, times=2):
    """Matnni takrorlash"""
    return text * times

@app.template_filter('currency')
def currency_filter(amount, currency='so\'m'):
    """Pul formatida ko'rsatish"""
    return f"{amount:,.0f} {currency}"
```

```html
<p>{{ "Hi! " | repeat(3) }}</p>  
<!-- Hi! Hi! Hi!  -->

<p>{{ 1500000 | currency }}</p>  
<!-- 1,500,000 so'm -->

<p>{{ 100 | currency('USD') }}</p>  
<!-- 100 USD -->
```

### 6.4. Amaliy Custom Filter misollari

**1. Vaqtni o'zbek tilida ko'rsatish:**
```python
from datetime import datetime

@app.template_filter('uzbek_date')
def uzbek_date_filter(date):
    """Sanani o'zbek formatida ko'rsatish"""
    months = {
        1: 'Yanvar', 2: 'Fevral', 3: 'Mart', 4: 'Aprel',
        5: 'May', 6: 'Iyun', 7: 'Iyul', 8: 'Avgust',
        9: 'Sentabr', 10: 'Oktabr', 11: 'Noyabr', 12: 'Dekabr'
    }
    
    if isinstance(date, str):
        date = datetime.strptime(date, '%Y-%m-%d')
    
    return f"{date.day} {months[date.month]} {date.year} yil"
```

```html
<p>{{ "2024-03-15" | uzbek_date }}</p>
<!-- 15 Mart 2024 yil -->
```

**2. Telefon raqamni formatlash:**
```python
@app.template_filter('phone_format')
def phone_format_filter(phone):
    """Telefon raqamni formatlash: +998 90 123 45 67"""
    phone = phone.replace('+', '').replace(' ', '').replace('-', '')
    
    if len(phone) == 12 and phone.startswith('998'):
        return f"+{phone[:3]} {phone[3:5]} {phone[5:8]} {phone[8:10]} {phone[10:]}"
    return phone
```

```html
<p>{{ "998901234567" | phone_format }}</p>
<!-- +998 90 123 45 67 -->
```

**3. Matn qisqartirish (truncate improved):**
```python
@app.template_filter('smart_truncate')
def smart_truncate_filter(text, length=100, suffix='...'):
    """Matnni so'z chegarasida qisqartirish"""
    if len(text) <= length:
        return text
    
    # So'zni kesmaslik uchun oxirgi bo'sh joyni topish
    truncated = text[:length].rsplit(' ', 1)[0]
    return truncated + suffix
```

```html
<p>{{ long_text | smart_truncate(50) }}</p>
```

**4. File hajmini ko'rsatish:**
```python
@app.template_filter('file_size')
def file_size_filter(bytes):
    """File hajmini odam tushunadigan formatda ko'rsatish"""
    for unit in ['B', 'KB', 'MB', 'GB', 'TB']:
        if bytes < 1024.0:
            return f"{bytes:.1f} {unit}"
        bytes /= 1024.0
    return f"{bytes:.1f} PB"
```

```html
<p>{{ 1048576 | file_size }}</p>  <!-- 1.0 MB -->
<p>{{ 1500 | file_size }}</p>     <!-- 1.5 KB -->
```

### 6.5. Multiple Filterlar zanjiri

```python
@app.template_filter('slugify')
def slugify_filter(text):
    """URL uchun slug yaratish"""
    import re
    text = text.lower()
    text = re.sub(r'[^\w\s-]', '', text)
    text = re.sub(r'[-\s]+', '-', text)
    return text.strip('-')
```

```html
<p>{{ "Python Dasturlash Tili" | slugify }}</p>
<!-- python-dasturlash-tili -->

{# Bir nechta filterlarni ketma-ket #}
<p>{{ "Python Dasturlash" | slugify | upper }}</p>
<!-- PYTHON-DASTURLASH -->
```

---

## Jinja'da Xavfsizlik

### 7.1. Autoescape nima?

**Autoescape** - Jinja avtomatik ravishda HTML maxsus belgilarni kodlaydi, shunda XSS (Cross-Site Scripting) hujumlaridan himoya bo'ladi.

**HTML maxsus belgilar:**
- `<` → `&lt;`
- `>` → `&gt;`
- `&` → `&amp;`
- `"` → `&quot;`
- `'` → `&#39;`

### 7.2. XSS hujumi nima? (xavfsiz tushuntirish)

**XSS (Cross-Site Scripting)** - bu haker foydalanuvchi kiritgan ma'lumotlar orqali zararli JavaScript kodni saytga qo'shish usulidir.

**Misol:**
```python
# Foydalanuvchi izoh yozdi
comment = "<script>alert('Hacked!')</script>"

# Agar autoescape bo'lmasa:
# Sahifada bu skript bajariladi va foydalanuvchiga xabar chiqadi
```

**Flask'da autoescape default ravishda yoqilgan:**
```html
{{ user_comment }}
<!-- Agar comment = "<script>alert('XSS')</script>" bo'lsa -->
<!-- Natija: &lt;script&gt;alert('XSS')&lt;/script&gt; -->
<!-- Bu xavfsiz, chunki skript bajarilmaydi -->
```

### 7.3. `|safe` filteri - Ehtiyot choralari

**⚠️ `|safe` filterini faqat ishonchli ma'lumotlarda ishlating!**

```python
@app.route('/article')
def article():
    # Bu HTML content admin tomonidan yozilgan, xavfsiz
    content = """
    <h2>Sarlavha</h2>
    <p>Bu <strong>muhim</strong> matn.</p>
    """
    return render_template('article.html', content=content)
```

```html
<!-- ❌ Yomon - HTML ko'rsatilmaydi -->
{{ content }}
<!-- Natija: &lt;h2&gt;Sarlavha&lt;/h2&gt;... -->

<!-- ✅ Yaxshi - HTML ishlaydi -->
{{ content | safe }}
<!-- Natija: <h2>Sarlavha</h2><p>Bu <strong>muhim</strong> matn.</p> -->
```

**⚠️ Hech qachon foydalanuvchi kiritgan ma'lumotga `|safe` ishlatmang:**
```html
<!-- ❌ JUDA XAVFLI! -->
{{ user_comment | safe }}

<!-- ✅ XAVFSIZ -->
{{ user_comment }}
```

### 7.4. Autoescape'ni o'chirish va yoqish

```html
{# Autoescape'ni vaqtinchalik o'chirish #}
{% autoescape false %}
    {{ html_content }}  {# Bu xavfli bo'lishi mumkin! #}
{% endautoescape %}

{# Autoescape'ni aniq yoqish #}
{% autoescape true %}
    {{ user_input }}  {# Xavfsiz #}
{% endautoescape %}
```

### 7.5. Xavfsiz HTML yaratish

**✅ To'g'ri yondashuv - Backend'da tozalash:**
```python
from markupsafe import escape

@app.route('/comment', methods=['POST'])
def post_comment():
    user_comment = request.form.get('comment')
    
    # HTML teglarni olib tashlash
    clean_comment = escape(user_comment)
    
    # Yoki faqat ma'lum teglarni ruxsat berish
    allowed_tags = ['b', 'i', 'u', 'a']
    clean_comment = clean_html(user_comment, allowed_tags)
    
    # Database'ga saqlash
    save_comment(clean_comment)
    
    return redirect('/')
```

### 7.6. Markdown bilan ishlash (xavfsiz)

```python
import markdown
from markupsafe import Markup

@app.template_filter('markdown')
def markdown_filter(text):
    """Markdown'ni xavfsiz HTML'ga o'girish"""
    # Markdown'ni HTML'ga o'girish
    html = markdown.markdown(text, extensions=['extra', 'codehilite'])
    
    # Markup ob'ekti yaratish (autoescape'dan o'tkazish)
    return Markup(html)
```

```html
<div class="article-content">
    {{ article.content | markdown }}
</div>
```

### 7.7. Xavfsizlik qoidalari

**✅ Qilish kerak:**
1. Foydalanuvchi kiritgan ma'lumotlarni hech qachon `|safe` bilan ishlatmaslik
2. Backend'da ma'lumotlarni validatsiya qilish
3. HTML kontentni faqat ishonchli manbadan qabul qilish
4. Markdown yoki boshqa markup tillarini ishlatish (to'g'ri tozalangan holda)

**❌ Qilmaslik kerak:**
1. `autoescape=False` qo'yish (agar juda kerak bo'lmasa)
2. Foydalanuvchi kiritgan ma'lumotga `|safe` qo'llash
3. SQL injection, XSS hujumlariga yo'l beruvchi kod yozish

---

## To'liq Mini-Loyiha: Django + DTL Blog

### 8.1. Loyiha tuzilishi

```
django_blog/
│
├── venv/                      # Virtual environment
│
├── blog_project/              # Django loyiha sozlamalari
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── blog/                      # Blog ilovasi
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   └── templatetags/         # Custom template tags va filters
│       ├── __init__.py
│       └── blog_tags.py
│
├── static/                    # Static fayllar
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── main.js
│   └── images/
│       └── logo.png
│
├── templates/                 # Django template'lar
│   ├── base.html             # Asosiy shablon
│   ├── blog/
│   │   ├── index.html        # Bosh sahifa
│   │   ├── article.html      # Maqola sahifasi
│   │   ├── about.html        # Biz haqimizda
│   │   └── category.html     # Kategoriya sahifasi
│   └── components/           # Qayta ishlatiladigan komponentlar
│       ├── header.html
│       ├── footer.html
│       └── article_card.html
│
└── manage.py                  # Django boshqaruv skripti
```

### 8.2. Django loyihasini yaratish

**1. Virtual environment va Django o'rnatish:**
```bash
# Windows
python -m venv venv
venv\Scripts\activate
pip install Django

# Linux/MacOS
python3 -m venv venv
source venv/bin/activate
pip install Django
```

**2. Django loyihasini yaratish:**
```bash
django-admin startproject blog_project .
python manage.py startapp blog
```

### 8.3. settings.py - Sozlamalar

```python
# blog_project/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',  # Blog ilovasini qo'shish
]

# Templates sozlamasi
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],  # Templates papkasi
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# Static fayllar
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']

# Til va vaqt zonasi
LANGUAGE_CODE = 'uz'
TIME_ZONE = 'Asia/Tashkent'
```

### 8.4. models.py - Ma'lumotlar modeli

```python
# blog/models.py
from django.db import models
from django.urls import reverse

class Category(models.Model):
    """Kategoriya modeli"""
    name = models.CharField(max_length=100, verbose_name="Nomi")
    slug = models.SlugField(unique=True)

    class Meta:
        verbose_name = "Kategoriya"
        verbose_name_plural = "Kategoriyalar"

    def __str__(self):
        return self.name

    def get_absolute_url(self):
        return reverse('blog:category', kwargs={'slug': self.slug})


class Article(models.Model):
    """Maqola modeli"""
    title = models.CharField(max_length=200, verbose_name="Sarlavha")
    slug = models.SlugField(unique=True)
    content = models.TextField(verbose_name="Matn")
    author = models.CharField(max_length=100, verbose_name="Muallif")
    category = models.ForeignKey(
        Category,
        on_delete=models.CASCADE,
        related_name='articles',
        verbose_name="Kategoriya"
    )
    image = models.ImageField(
        upload_to='articles/',
        blank=True,
        null=True,
        verbose_name="Rasm"
    )
    views = models.PositiveIntegerField(default=0, verbose_name="Ko'rishlar")
    created_at = models.DateTimeField(auto_now_add=True, verbose_name="Yaratilgan sana")
    updated_at = models.DateTimeField(auto_now=True, verbose_name="Yangilangan sana")

    class Meta:
        verbose_name = "Maqola"
        verbose_name_plural = "Maqolalar"
        ordering = ['-created_at']

    def __str__(self):
        return self.title

    def get_absolute_url(self):
        return reverse('blog:article', kwargs={'slug': self.slug})


class TeamMember(models.Model):
    """Jamoa a'zosi modeli"""
    name = models.CharField(max_length=100, verbose_name="Ism")
    role = models.CharField(max_length=100, verbose_name="Lavozim")
    bio = models.TextField(verbose_name="Biografiya")

    class Meta:
        verbose_name = "Jamoa a'zosi"
        verbose_name_plural = "Jamoa a'zolari"

    def __str__(self):
        return self.name
```

### 8.5. views.py - Django Views

```python
# blog/views.py
from django.shortcuts import render, get_object_or_404
from django.db.models import Sum, Count
from .models import Article, Category, TeamMember


def index(request):
    """Bosh sahifa"""
    articles = Article.objects.select_related('category').all()
    total_articles = articles.count()
    total_views = articles.aggregate(total=Sum('views'))['total'] or 0
    total_authors = articles.values('author').distinct().count()
    categories = Category.objects.annotate(article_count=Count('articles'))

    context = {
        'articles': articles,
        'total_articles': total_articles,
        'total_views': total_views,
        'total_authors': total_authors,
        'categories': categories,
    }
    return render(request, 'blog/index.html', context)


def article_detail(request, slug):
    """Maqola sahifasi"""
    article = get_object_or_404(Article.objects.select_related('category'), slug=slug)

    # Ko'rishlar sonini oshirish
    article.views += 1
    article.save(update_fields=['views'])

    # Tegishli maqolalar (bir xil kategoriyadan)
    related_articles = Article.objects.filter(
        category=article.category
    ).exclude(id=article.id)[:3]

    context = {
        'article': article,
        'related_articles': related_articles,
    }
    return render(request, 'blog/article.html', context)


def about(request):
    """Biz haqimizda sahifasi"""
    team = TeamMember.objects.all()

    stats = {
        'articles': Article.objects.count(),
        'authors': team.count(),
        'readers': '10k+',
        'years': 2,
    }

    context = {
        'team': team,
        'stats': stats,
    }
    return render(request, 'blog/about.html', context)


def category_view(request, slug):
    """Kategoriya bo'yicha maqolalar"""
    category = get_object_or_404(Category, slug=slug)
    articles = Article.objects.filter(category=category)

    context = {
        'category': category,
        'articles': articles,
    }
    return render(request, 'blog/category.html', context)
```

### 8.6. urls.py - URL sozlamalari

```python
# blog_project/urls.py (asosiy)
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls', namespace='blog')),
]

# Development uchun media fayllar
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

```python
# blog/urls.py (ilova)
from django.urls import path
from . import views

app_name = 'blog'

urlpatterns = [
    path('', views.index, name='index'),
    path('article/<slug:slug>/', views.article_detail, name='article'),
    path('about/', views.about, name='about'),
    path('category/<slug:slug>/', views.category_view, name='category'),
]
```

### 8.7. Custom Template Tags va Filters

```python
# blog/templatetags/blog_tags.py
from django import template
from django.utils import timezone

register = template.Library()


@register.filter(name='uzbek_date')
def uzbek_date(date_obj):
    """Sanani o'zbek formatida ko'rsatish"""
    if not date_obj:
        return ''

    months = {
        1: 'Yanvar', 2: 'Fevral', 3: 'Mart', 4: 'Aprel',
        5: 'May', 6: 'Iyun', 7: 'Iyul', 8: 'Avgust',
        9: 'Sentabr', 10: 'Oktabr', 11: 'Noyabr', 12: 'Dekabr'
    }

    return f"{date_obj.day} {months[date_obj.month]} {date_obj.year}"


@register.filter(name='format_views')
def format_views(views):
    """Ko'rishlar sonini k, M formatda ko'rsatish"""
    if views >= 1000000:
        return f"{views/1000000:.1f}M"
    elif views >= 1000:
        return f"{views/1000:.1f}k"
    return str(views)


@register.simple_tag
def current_year():
    """Joriy yilni qaytarish"""
    return timezone.now().year
```

### 8.8. base.html - Asosiy Shablon

```html
<!-- templates/base.html -->
{% load static %}
{% load blog_tags %}
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    {# Har bir sahifa o'z title'ini beradi #}
    <title>{% block title %}Blog{% endblock %} - Dasturlash Blogi</title>

    {# CSS #}
    <link rel="stylesheet" href="{% static 'css/style.css' %}">

    {# Qo'shimcha CSS - ba'zi sahifalar uchun #}
    {% block extra_css %}{% endblock %}
</head>
<body>
    {# Header - barcha sahifalarda bir xil #}
    {% include 'components/header.html' %}

    {# Main content - har bir sahifa o'zgartiradi #}
    <main class="container">
        {% block content %}
        <!-- Default content -->
        {% endblock %}
    </main>

    {# Footer - barcha sahifalarda bir xil #}
    {% include 'components/footer.html' %}

    {# JavaScript #}
    <script src="{% static 'js/main.js' %}"></script>

    {# Qo'shimcha JavaScript #}
    {% block extra_js %}{% endblock %}
</body>
</html>
```

### 8.9. components/header.html

```html
<!-- templates/components/header.html -->
{% load static %}
<header class="site-header">
    <div class="container">
        <div class="header-content">
            {# Logo #}
            <div class="logo">
                <a href="{% url 'blog:index' %}">
                    <img src="{% static 'images/logo.png' %}" alt="Logo">
                    <h1>Dasturlash Blogi</h1>
                </a>
            </div>

            {# Navigation #}
            <nav class="main-nav">
                <ul>
                    <li>
                        <a href="{% url 'blog:index' %}"
                           class="{% if request.resolver_match.url_name == 'index' %}active{% endif %}">
                            Bosh sahifa
                        </a>
                    </li>
                    <li>
                        <a href="{% url 'blog:category' slug='dasturlash' %}">
                            Dasturlash
                        </a>
                    </li>
                    <li>
                        <a href="{% url 'blog:category' slug='web-development' %}">
                            Web Development
                        </a>
                    </li>
                    <li>
                        <a href="{% url 'blog:about' %}"
                           class="{% if request.resolver_match.url_name == 'about' %}active{% endif %}">
                            Biz haqimizda
                        </a>
                    </li>
                </ul>
            </nav>

            {# Search bar #}
            <div class="search-bar">
                <form action="{% url 'blog:index' %}" method="GET">
                    <input type="text" name="q" placeholder="Qidirish...">
                    <button type="submit">🔍</button>
                </form>
            </div>
        </div>
    </div>
</header>
```

### 8.10. components/footer.html

```html
<!-- templates/components/footer.html -->
{% load blog_tags %}
<footer class="site-footer">
    <div class="container">
        <div class="footer-content">
            {# Blog haqida #}
            <div class="footer-section">
                <h3>Dasturlash Blogi</h3>
                <p>Python, Flask, Django va web-dasturlash bo'yicha maqolalar.</p>
                <div class="social-links">
                    <a href="#" title="Telegram">📱</a>
                    <a href="#" title="Instagram">📷</a>
                    <a href="#" title="YouTube">📺</a>
                </div>
            </div>

            {# Tezkor havolalar #}
            <div class="footer-section">
                <h3>Tezkor havolalar</h3>
                <ul>
                    <li><a href="{% url 'blog:index' %}">Bosh sahifa</a></li>
                    <li><a href="{% url 'blog:about' %}">Biz haqimizda</a></li>
                    <li><a href="#">Aloqa</a></li>
                    <li><a href="#">Maxfiylik siyosati</a></li>
                </ul>
            </div>

            {# Kategoriyalar #}
            <div class="footer-section">
                <h3>Kategoriyalar</h3>
                <ul>
                    <li><a href="{% url 'blog:category' slug='dasturlash' %}">Dasturlash</a></li>
                    <li><a href="{% url 'blog:category' slug='web-development' %}">Web Development</a></li>
                </ul>
            </div>

            {# Aloqa #}
            <div class="footer-section">
                <h3>Aloqa</h3>
                <p>📧 info@blog.uz</p>
                <p>📞 +998 90 123 45 67</p>
                <p>📍 Toshkent, O'zbekiston</p>
            </div>
        </div>

        <div class="footer-bottom">
            <p>&copy; {% current_year %} Dasturlash Blogi. Barcha huquqlar himoyalangan.</p>
        </div>
    </div>
</footer>
```

### 8.11. components/article_card.html

```html
<!-- templates/components/article_card.html -->
{% load static blog_tags %}
<article class="article-card">
    {# Maqola rasmi #}
    <div class="article-image">
        <a href="{{ article.get_absolute_url }}">
            {% if article.image %}
                <img src="{{ article.image.url }}" alt="{{ article.title }}">
            {% else %}
                <img src="{% static 'images/default.jpg' %}" alt="{{ article.title }}">
            {% endif %}
        </a>
        <span class="category-badge">{{ article.category.name }}</span>
    </div>

    {# Maqola ma'lumotlari #}
    <div class="article-content">
        <h3>
            <a href="{{ article.get_absolute_url }}">
                {{ article.title }}
            </a>
        </h3>

        <p class="article-excerpt">
            {{ article.content|truncatewords:25 }}
        </p>

        <div class="article-meta">
            <span class="author">👤 {{ article.author }}</span>
            <span class="date">📅 {{ article.created_at|uzbek_date }}</span>
            <span class="views">👁 {{ article.views|format_views }}</span>
        </div>

        <a href="{{ article.get_absolute_url }}" class="read-more">
            Davomini o'qish →
        </a>
    </div>
</article>
```

### 8.12. index.html - Bosh Sahifa

```html
<!-- templates/blog/index.html -->
{% extends "base.html" %}
{% load blog_tags %}

{% block title %}Bosh sahifa{% endblock %}

{% block content %}
<div class="home-page">
    {# Hero section #}
    <section class="hero">
        <h1>Dasturlash Dunyosiga Xush Kelibsiz!</h1>
        <p>Python, Flask, Django va web-dasturlash bo'yicha eng so'nggi maqolalar</p>
    </section>

    {# Statistika #}
    <section class="stats">
        <div class="stat-item">
            <h2>{{ total_articles }}</h2>
            <p>Maqolalar</p>
        </div>
        <div class="stat-item">
            <h2>{{ total_views|format_views }}</h2>
            <p>Ko'rishlar</p>
        </div>
        <div class="stat-item">
            <h2>{{ total_authors }}</h2>
            <p>Mualliflar</p>
        </div>
    </section>

    {# Maqolalar grid #}
    <section class="articles-section">
        <h2>Eng So'nggi Maqolalar</h2>

        {% if articles %}
            <div class="articles-grid">
            {% for article in articles %}
                {% include 'components/article_card.html' with article=article %}
            {% endfor %}
            </div>
        {% else %}
            <p class="no-articles">Hozircha maqolalar yo'q.</p>
        {% endif %}
    </section>

    {# Kategoriyalar bo'yicha #}
    <section class="categories-section">
        <h2>Kategoriyalar</h2>

        <div class="categories-list">
        {% for category in categories %}
            <a href="{{ category.get_absolute_url }}" class="category-item">
                <h3>{{ category.name }}</h3>
                <p>{{ category.article_count }} ta maqola</p>
            </a>
        {% endfor %}
        </div>
    </section>
</div>
{% endblock %}
```

**Tushuntirish (Flask vs Django farqlari):**

| Flask (Jinja2) | Django (DTL) |
|----------------|--------------|
| `{{ url_for('index') }}` | `{% url 'blog:index' %}` |
| `{{ url_for('static', filename='css/style.css') }}` | `{% static 'css/style.css' %}` |
| `{% include 'component.html' %}` | `{% include 'component.html' with var=value %}` |
| `{{ text \| truncate(150) }}` | `{{ text\|truncatewords:25 }}` |
| `request.endpoint` | `request.resolver_match.url_name` |

### 8.13. article.html - Maqola Sahifasi

```html
<!-- templates/blog/article.html -->
{% extends "base.html" %}
{% load blog_tags %}

{% block title %}{{ article.title }}{% endblock %}

{% block extra_css %}
<style>
    .article-page {
        max-width: 800px;
        margin: 0 auto;
    }
    .article-header {
        margin-bottom: 2rem;
    }
    .article-body {
        line-height: 1.8;
        font-size: 1.1rem;
    }
</style>
{% endblock %}

{% block content %}
<div class="article-page">
    {# Maqola header #}
    <header class="article-header">
        <div class="breadcrumb">
            <a href="{% url 'blog:index' %}">Bosh sahifa</a> /
            <a href="{{ article.category.get_absolute_url }}">
                {{ article.category.name }}
            </a> /
            <span>{{ article.title }}</span>
        </div>

        <h1>{{ article.title }}</h1>

        <div class="article-meta">
            <div class="meta-item">
                <strong>Muallif:</strong> {{ article.author }}
            </div>
            <div class="meta-item">
                <strong>Sana:</strong> {{ article.created_at|uzbek_date }}
            </div>
            <div class="meta-item">
                <strong>Kategoriya:</strong>
                <a href="{{ article.category.get_absolute_url }}">
                    {{ article.category.name }}
                </a>
            </div>
            <div class="meta-item">
                <strong>Ko'rishlar:</strong> {{ article.views|format_views }}
            </div>
        </div>
    </header>

    {# Maqola rasmi #}
    {% if article.image %}
    <div class="article-featured-image">
        <img src="{{ article.image.url }}" alt="{{ article.title }}">
    </div>
    {% endif %}

    {# Maqola matni #}
    <div class="article-body">
        {{ article.content|linebreaks }}
    </div>

    {# Ijtimoiy tarmoqlarda ulashish #}
    <div class="share-section">
        <h3>Ulashing:</h3>
        <div class="share-buttons">
            <button onclick="shareToTelegram()">📱 Telegram</button>
            <button onclick="shareToFacebook()">📘 Facebook</button>
            <button onclick="copyLink()">🔗 Link nusxalash</button>
        </div>
    </div>

    {# Tegishli maqolalar #}
    {% if related_articles %}
    <section class="related-articles">
        <h2>Shunga o'xshash maqolalar</h2>
        <div class="related-grid">
        {% for article in related_articles %}
            {% include 'components/article_card.html' with article=article %}
        {% endfor %}
        </div>
    </section>
    {% endif %}
</div>
{% endblock %}

{% block extra_js %}
<script>
    function shareToTelegram() {
        const url = window.location.href;
        const text = "{{ article.title }}";
        window.open(`https://t.me/share/url?url=${url}&text=${text}`);
    }

    function copyLink() {
        navigator.clipboard.writeText(window.location.href);
        alert('Link nusxalandi!');
    }
</script>
{% endblock %}
```

### 8.14. about.html - Biz haqimizda

```html
<!-- templates/blog/about.html -->
{% extends "base.html" %}

{% block title %}Biz haqimizda{% endblock %}

{% block content %}
<div class="about-page">
    <h1>Biz haqimizda</h1>

    {# Statistika #}
    <section class="stats">
        <div class="stat-item">
            <h2>{{ stats.articles }}</h2>
            <p>Maqolalar</p>
        </div>
        <div class="stat-item">
            <h2>{{ stats.authors }}</h2>
            <p>Mualliflar</p>
        </div>
        <div class="stat-item">
            <h2>{{ stats.readers }}</h2>
            <p>O'quvchilar</p>
        </div>
        <div class="stat-item">
            <h2>{{ stats.years }}</h2>
            <p>Yillik tajriba</p>
        </div>
    </section>

    {# Jamoa a'zolari #}
    <section class="team-section">
        <h2>Bizning Jamoa</h2>
        <div class="team-grid">
        {% for member in team %}
            <div class="team-member">
                <h3>{{ member.name }}</h3>
                <p class="role">{{ member.role }}</p>
                <p class="bio">{{ member.bio }}</p>
            </div>
        {% empty %}
            <p>Jamoa a'zolari haqida ma'lumot yo'q.</p>
        {% endfor %}
        </div>
    </section>
</div>
{% endblock %}
```

### 8.15. category.html - Kategoriya sahifasi

```html
<!-- templates/blog/category.html -->
{% extends "base.html" %}

{% block title %}{{ category.name }}{% endblock %}

{% block content %}
<div class="category-page">
    <h1>{{ category.name }}</h1>
    <p class="category-description">{{ category.name }} kategoriyasidagi barcha maqolalar</p>

    {% if articles %}
        <div class="articles-grid">
        {% for article in articles %}
            {% include 'components/article_card.html' with article=article %}
        {% endfor %}
        </div>
    {% else %}
        <p class="no-articles">Bu kategoriyada hozircha maqolalar yo'q.</p>
    {% endif %}
</div>
{% endblock %}
```

### 8.16. static/css/style.css

```css
/* static/css/style.css */

/* Global styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #333;
    background-color: #f4f4f4;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

/* Header */
.site-header {
    background-color: #2c3e50;
    color: white;
    padding: 1rem 0;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
}

.logo {
    display: flex;
    align-items: center;
    gap: 10px;
}

.logo a {
    color: white;
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 10px;
}

.logo img {
    width: 40px;
    height: 40px;
}

.main-nav ul {
    list-style: none;
    display: flex;
    gap: 20px;
}

.main-nav a {
    color: white;
    text-decoration: none;
    padding: 0.5rem 1rem;
    border-radius: 5px;
    transition: background-color 0.3s;
}

.main-nav a:hover,
.main-nav a.active {
    background-color: #34495e;
}

/* Search bar */
.search-bar form {
    display: flex;
}

.search-bar input {
    padding: 0.5rem;
    border: none;
    border-radius: 5px 0 0 5px;
    width: 200px;
}

.search-bar button {
    padding: 0.5rem 1rem;
    border: none;
    background-color: #3498db;
    color: white;
    border-radius: 0 5px 5px 0;
    cursor: pointer;
}

/* Main content */
main {
    min-height: calc(100vh - 200px);
    padding: 2rem 0;
}

/* Hero section */
.hero {
    text-align: center;
    padding: 3rem 0;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    border-radius: 10px;
    margin-bottom: 2rem;
}

.hero h1 {
    font-size: 2.5rem;
    margin-bottom: 1rem;
}

/* Stats */
.stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
    margin-bottom: 2rem;
}

.stat-item {
    background: white;
    padding: 2rem;
    text-align: center;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.stat-item h2 {
    font-size: 2.5rem;
    color: #3498db;
}

/* Articles grid */
.articles-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 30px;
    margin-top: 2rem;
}

/* Article card */
.article-card {
    background: white;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    transition: transform 0.3s, box-shadow 0.3s;
}

.article-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 5px 20px rgba(0,0,0,0.2);
}

.article-image {
    position: relative;
    height: 200px;
    overflow: hidden;
}

.article-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.category-badge {
    position: absolute;
    top: 10px;
    right: 10px;
    background-color: #3498db;
    color: white;
    padding: 0.25rem 0.75rem;
    border-radius: 20px;
    font-size: 0.85rem;
}

.article-content {
    padding: 1.5rem;
}

.article-content h3 {
    margin-bottom: 1rem;
}

.article-content h3 a {
    color: #2c3e50;
    text-decoration: none;
}

.article-content h3 a:hover {
    color: #3498db;
}

.article-excerpt {
    color: #666;
    margin-bottom: 1rem;
}

.article-meta {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    font-size: 0.85rem;
    color: #999;
    margin-bottom: 1rem;
}

.read-more {
    display: inline-block;
    color: #3498db;
    text-decoration: none;
    font-weight: bold;
}

.read-more:hover {
    text-decoration: underline;
}

/* Footer */
.site-footer {
    background-color: #2c3e50;
    color: white;
    padding: 3rem 0 1rem;
    margin-top: 3rem;
}

.footer-content {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 30px;
    margin-bottom: 2rem;
}

.footer-section h3 {
    margin-bottom: 1rem;
    color: #3498db;
}

.footer-section ul {
    list-style: none;
}

.footer-section a {
    color: #bdc3c7;
    text-decoration: none;
}

.footer-section a:hover {
    color: white;
}

.social-links {
    display: flex;
    gap: 10px;
    font-size: 1.5rem;
}

.footer-bottom {
    text-align: center;
    padding-top: 1rem;
    border-top: 1px solid #34495e;
}

/* Team section */
.team-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 30px;
    margin-top: 2rem;
}

.team-member {
    background: white;
    padding: 2rem;
    border-radius: 10px;
    text-align: center;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.team-member .role {
    color: #3498db;
    font-weight: bold;
    margin-bottom: 1rem;
}

/* Responsive */
@media (max-width: 768px) {
    .header-content {
        flex-direction: column;
        gap: 1rem;
    }

    .main-nav ul {
        flex-direction: column;
        gap: 0;
    }

    .hero h1 {
        font-size: 1.8rem;
    }

    .articles-grid {
        grid-template-columns: 1fr;
    }
}
```

### 8.17. admin.py - Admin panel

```python
# blog/admin.py
from django.contrib import admin
from .models import Category, Article, TeamMember


@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    list_display = ['name', 'slug']
    prepopulated_fields = {'slug': ('name',)}


@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    list_display = ['title', 'author', 'category', 'views', 'created_at']
    list_filter = ['category', 'created_at']
    search_fields = ['title', 'content', 'author']
    prepopulated_fields = {'slug': ('title',)}
    date_hierarchy = 'created_at'


@admin.register(TeamMember)
class TeamMemberAdmin(admin.ModelAdmin):
    list_display = ['name', 'role']
```

### 8.18. Loyihani ishga tushurish

**1. Virtual environment yaratish va Django o'rnatish:**
```bash
# Windows
python -m venv venv
venv\Scripts\activate
pip install Django Pillow

# Linux/MacOS
python3 -m venv venv
source venv/bin/activate
pip install Django Pillow
```

**2. Ma'lumotlar bazasini yaratish:**
```bash
python manage.py makemigrations
python manage.py migrate
```

**3. Admin foydalanuvchi yaratish:**
```bash
python manage.py createsuperuser
```

**4. Loyihani ishga tushurish:**
```bash
python manage.py runserver
```

**5. Brauzerda ochish:**
```
http://127.0.0.1:8000        # Bosh sahifa
http://127.0.0.1:8000/admin  # Admin panel
```

### 8.19. Flask vs Django Template farqlari jadvali

| Xususiyat | Flask (Jinja2) | Django (DTL) |
|-----------|----------------|--------------|
| **URL yaratish** | `{{ url_for('view_name') }}` | `{% url 'app:view_name' %}` |
| **Static fayllar** | `{{ url_for('static', filename='...') }}` | `{% static '...' %}` |
| **Filter ishlatish** | `{{ var \| filter }}` | `{{ var\|filter }}` (bo'shliqsiz) |
| **Matnni qisqartirish** | `{{ text \| truncate(150) }}` | `{{ text\|truncatewords:25 }}` |
| **Ro'yxat bo'sh bo'lsa** | `{% else %}` (for ichida) | `{% empty %}` (for ichida) |
| **Custom filter** | `@app.template_filter()` | `@register.filter()` |
| **Include bilan o'zgaruvchi** | `{% include '...' %}` (kontekst avtomatik) | `{% include '...' with var=value %}` |
| **Joriy yil** | `{{ "now"\|date("%Y") }}` | `{% now "Y" %}` yoki custom tag |

---

## O'quvchilar Uchun Amaliy Topshiriqlar

### Topshiriq 1: Oddiy Portfolio Sahifa

**Vazifa:** Template inheritance ishlatib, shaxsiy portfolio sahifa yarating.

**Talablar:**
- `base.html` - header, footer bilan
- `index.html` - "Men haqimda" sahifa
- `projects.html` - Loyihalar ro'yxati (for loop ishlatish)
- `contact.html` - Aloqa forma

**Ko'rsatma:**
```python
# app.py
@app.route('/projects')
def projects():
    my_projects = [
        {'name': 'Blog', 'tech': 'Flask', 'year': 2024},
        {'name': 'Todo App', 'tech': 'Django', 'year': 2023},
        # ...
    ]
    return render_template('projects.html', projects=my_projects)
```

### Topshiriq 2: Ro'yxat Filtrlash

**Vazifa:** Mahsulotlar ro'yxatini kategoriya va narx bo'yicha filter qilish.

**Talablar:**
- Mahsulotlar list'i
- Kategoriya bo'yicha filter
- Narx oralig'i bo'yicha filter
- For loop va if shartlari ishlatish

**Misol kod:**
```python
@app.route('/products')
def products():
    products = [
        {'name': 'Laptop', 'price': 5000000, 'category': 'Elektronika'},
        {'name': 'Kitob', 'price': 50000, 'category': 'Adabiyot'},
        # ...
    ]
    
    category = request.args.get('category')
    min_price = request.args.get('min_price', type=int)
    
    return render_template('products.html', products=products)
```

### Topshiriq 3: Custom Filter - So'm Formati

**Vazifa:** Narxlarni o'zbek so'm formatida ko'rsatadigan custom filter yarating.

**Talablar:**
- Filter nomi: `uzs_format`
- Input: 1500000
- Output: "1 500 000 so'm"

**Kod skelet:**
```python
@app.template_filter('uzs_format')
def uzs_format_filter(amount):
    # Bu yerni to'ldiring
    pass
```

### Topshiriq 4: Macro - Card Component

**Vazifa:** Qayta ishlatiladigan card macro yarating.

**Talablar:**
- Parametrlar: title, content, image, link
- Default qiymatlar bo'lsin
- Kamida 3 ta turli joyda ishlatilsin

**Misol:**
```html
{% macro card(title, content, image='', link='#') %}
    <!-- Bu yerni to'ldiring -->
{% endmacro %}
```

### Topshiriq 5: Conditional Rendering - User Dashboard

**Vazifa:** Foydalanuvchi tipi bo'yicha turli kontent ko'rsatish.

**Talablar:**
- Admin foydalanuvchiga admin panel
- Oddiy foydalanuvchiga oddiy dashboard
- Mehmon (login qilmagan) ga login sahifa

**Ko'rsatma:**
```html
{% if user %}
    {% if user.is_admin %}
        <!-- Admin panel -->
    {% else %}
        <!-- User dashboard -->
    {% endif %}
{% else %}
    <!-- Login page -->
{% endif %}
```

### Topshiriq 6: Dynamic HTML Table

**Vazifa:** Ma'lumotlar bazasidan kelgan ma'lumotlarni jadvalda ko'rsatish.

**Talablar:**
- Table header dinamik
- Qatorlar for loop bilan
- loop.index, loop.first, loop.last ishlatish
- Bo'sh table uchun xabar

**Misol:**
```python
students = [
    {'id': 1, 'name': 'Ali', 'grade': 85, 'status': 'active'},
    # ...
]
```

### Topshiriq 7: Qidiruv Funksiyasi

**Vazifa:** Maqolalar orasida qidiruv.

**Talablar:**
- Search input
- Query bo'yicha maqolalarni filter qilish
- Natijalar sonini ko'rsatish
- Topilmasa "Natija topilmadi" xabari

**Ko'rsatma:**
```python
@app.route('/search')
def search():
    query = request.args.get('q', '').lower()
    results = [a for a in articles if query in a['title'].lower()]
    return render_template('search.html', results=results, query=query)
```

### Topshiriq 8: Blog Comments System

**Vazifa:** Maqolaga izoh qoldirish tizimi (frontend qismi).

**Talablar:**
- Izoh forma (macro bilan)
- Izohlar ro'yxati (for loop)
- Izoh soni
- Javob berish imkoniyati (nested loop)

**Struktura:**
```python
comments = [
    {
        'id': 1,
        'author': 'Ali',
        'text': 'Ajoyib maqola!',
        'date': '2024-01-15',
        'replies': [
            {'author': 'Vali', 'text': 'Roziman!'}
        ]
    }
]
```

### Topshiriq 9: Multi-language Support

**Vazifa:** Saytni 2 tilda (o'zbek, ingliz) ko'rsatish.

**Talablar:**
- Language switcher
- Dictionary bilan tarjimalar
- Til bo'yicha kontent ko'rsatish

**Misol:**
```python
translations = {
    'uz': {
        'home': 'Bosh sahifa',
        'about': 'Biz haqimizda',
        # ...
    },
    'en': {
        'home': 'Home',
        'about': 'About Us',
        # ...
    }
}

@app.route('/<lang>')
def index(lang='uz'):
    return render_template('index.html', t=translations[lang])
```

### Topshiriq 10: Complete E-commerce Product Page

**Vazifa:** To'liq mahsulot sahifasi yarating.

**Talablar:**
1. **Mahsulot ma'lumotlari:**
   - Rasm galereyasi (for loop)
   - Narx (custom filter)
   - Tavsif
   - Xususiyatlar (dictionary)

2. **Tegishli mahsulotlar:**
   - 4 ta o'xshash mahsulot
   - Card component (macro yoki include)

3. **Sharhlar:**
   - Yulduzcha reytingi
   - Foydalanuvchi sharhlari
   - loop.first/last ishlatish

4. **Xavfsizlik:**
   - User input uchun autoescape
   - |safe filterni to'g'ri ishlatish

**Kengaytirilgan kod:**
```python
@app.route('/product/<int:product_id>')
def product(product_id):
    product = {
        'id': 1,
        'name': 'MacBook Pro 16"',
        'price': 15000000,
        'images': ['img1.jpg', 'img2.jpg', 'img3.jpg'],
        'description': 'Zamonaviy laptop',
        'specs': {
            'CPU': 'M3 Pro',
            'RAM': '32GB',
            'Storage': '1TB SSD'
        },
        'rating': 4.8,
        'reviews': [
            {'user': 'Ali', 'rating': 5, 'text': 'Ajoyib!'},
            # ...
        ]
    }
    
    related_products = [...]  # O'xshash mahsulotlar
    
    return render_template('product.html', 
                         product=product, 
                         related=related_products)
```

---

## Yakuniy qism

### 10.1. Jinja nimaga qulay?

**Jinja'ning kuchli tomonlari:**

1. **Sodda va tushunarli sintaksis**
   - Python'ga o'xshash
   - O'rganish oson
   - Kod o'qish qulay

2. **Template Inheritance**
   - DRY (Don't Repeat Yourself) prinsipi
   - Kodni qayta ishlat ish
   - Katta loyihalarda tartib

3. **Kuchli filter tizimi**
   - 40+ built-in filter
   - Custom filterlar yaratish oson
   - Zanjir (chaining) imkoniyati

4. **Xavfsizlik**
   - Autoescape default yoniq
   - XSS hujumlaridan himoya
   - Ishonchli va xavfsiz

5. **Moslashuvchanlik**
   - HTML, XML, LaTeX uchun
   - Email templatelar
   - Konfiguratsiya fayllar

### 10.2. Flask vs Django Template Engine

| Xususiyat | Jinja (Flask) | DTL (Django) |
|-----------|---------------|--------------|
| **Sintaksis** | `{{ variable }}` | `{{ variable }}` |
| **Filters** | 40+ built-in | 60+ built-in |
| **Template inheritance** | ✅ `extends` | ✅ `extends` |
| **Custom filterlar** | ✅ Oson | ✅ Oson |
| **Python code** | ❌ Yo'q | ❌ Yo'q |
| **Makrolar** | ✅ Ha | ❌ Yo'q |
| **Whitespace control** | ✅ Ha (`{%-`) | ❌ Cheklangan |
| **Autoescape** | ✅ Default on | ✅ Default on |
| **Performance** | Tez | Tez |
| **Dokumentatsiya** | Yaxshi | Zo'r |

**O'xshashliklar:**
- Sintaksis deyarli bir xil
- Template inheritance mavjud
- Filter tizimi o'xshash
- Xavfsizlik (autoescape)

**Farqlar:**
- Jinja'da macro bor, Django'da yo'q
- Django'da ko'proq built-in filtrlar
- Jinja mustaqil, Django'ga bog'liq emas

### 10.3. Katta loyihalarda qayerda ishlatiladi?

**1. Web Aplikatsiyalar:**
- Flask web-saytlar
- Admin panellar
- Dashboard'lar
- Blog platformalar

**2. Email Templatelar:**
```python
from flask import render_template
from flask_mail import Message

def send_welcome_email(user):
    html = render_template('emails/welcome.html', user=user)
    msg = Message('Xush kelibsiz!', 
                  recipients=[user.email],
                  html=html)
    mail.send(msg)
```

**3. PDF Generatsiya:**
```python
from weasyprint import HTML

html_string = render_template('invoice.html', invoice_data)
HTML(string=html_string).write_pdf('invoice.pdf')
```

**4. Static Site Generation:**
- Blog yaratish
- Dokumentatsiya saytlar
- Portfolio saytlar

**5. DevOps va Automation:**
- Ansible playbooks
- Konfiguratsiya fayllar
- Infrastructure as Code

### 10.4. Jinja vs Boshqa Template Enginelar

| Engine | Til | Ishlatilishi |
|--------|-----|-------------|
| **Jinja** | Python | Flask, Ansible |
| **Django Templates** | Python | Django |
| **Handlebars** | JavaScript | Frontend |
| **EJS** | JavaScript | Node.js |
| **Blade** | PHP | Laravel |
| **Thymeleaf** | Java | Spring |

### 10.5. Eng Muhim Narsalar

**Esda saqlash kerak:**

1. ✅ Template inheritance - eng kuchli xususiyat
2. ✅ Filtrlar - ma'lumotlarni formatlash
3. ✅ Macro - komponentlar yaratish
4. ✅ Autoescape - xavfsizlik
5. ✅ Loop obyekti - iteration'da yordam beradi

**Qilmaslik kerak:**

1. ❌ Template'da murakkab biznes logika
2. ❌ Foydalanuvchi input'ga `|safe` ishlatish
3. ❌ Har bir sahifada kodni takrorlash
4. ❌ Template'da database query

### 10.6. Keyingi Qadamlar

**Jinja'ni o'rgangandan keyin:**

1. **Flask** - to'liq web framework
2. **Django** - katta loyihalar uchun
3. **FastAPI** - zamonaviy API yaratish
4. **SQLAlchemy** - database bilan ishlash
5. **REST API** - backend development

### 10.7. Qo'shimcha Resurslar

**Rasmiy dokumentatsiya:**
- https://jinja.palletsprojects.com/
- https://flask.palletsprojects.com/
---