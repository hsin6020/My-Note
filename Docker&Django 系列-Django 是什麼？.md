
# Docker&Django 系列-Django 是什麼？
## Django
### 基本介紹
以 Python 作為基礎的網站開發框架
### 實作簡單的範例
#### 1. 建立一個虛擬環境，啟動虛擬環境，並安裝 django
```
# 以下是用 conda
> conda create -n Venv python=3.9
> conda activate Venv
> pip install django
```
#### 2. 建立 Django 專案
`> django-admin startproject mysite`
專案架構如下，黃框的檔案以下步驟會新增
![](https://i.imgur.com/q8IYmDk.png)

#### 3. 在 mysite/mysite 下建立 `views.py` 檔案
![](https://i.imgur.com/AfJEPHh.png)

```
# views.py
from django.shortcuts import render

def homepage(request):
    return render(request, 'index.html')
```

#### 3. 在 mysite/mysite 下建立 `templates` 資料夾，並在下面建立`index.html`
![](https://i.imgur.com/89WipFz.png)

#### 4. 在 mysite/mysite/settings.py 的 INSTALLED_APPS 裡加入 `'mysite'`
![](https://i.imgur.com/AVcKHPn.png)

#### 5. 在 mysite/mysite/urls.py 裡，import 剛剛寫好的 views，並加入 path，這樣就完成啦～
![](https://i.imgur.com/g9p20DB.png)

#### 6. 在 mysite 下 runserver 啟動專案
`> python manage.py runserver`

#### 7. 在瀏覽器打 127.0.0.1:8000 就可看到剛剛寫好的頁面了！
![](https://i.imgur.com/RwSHjek.png)

#### 為了讓其他人知道使用專案要先安裝哪些套件，我們會在 mysite 下製作 `requirements.txt` 放入套件，內容可以用指令取得
`> pip freeze`