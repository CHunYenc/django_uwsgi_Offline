# Nginx step
- [Nginx step](#nginx-step)
- [加入 nginx repos](#加入-nginx-repos)
- [前置作業](#前置作業)
  - [下載 repos](#下載-repos)
  - [下載 sqlite3.rpm](#下載-sqlite3rpm)
  - [所有的 rpm 檔](#所有的-rpm-檔)
  - [安裝](#安裝)
    - [到 yum-package](#到-yum-package)
    - [安裝 所有 rpm 檔案](#安裝-所有-rpm-檔案)
    - [查看 sqlite3 版本](#查看-sqlite3-版本)
    - [查看有沒有 python3](#查看有沒有-python3)
  - [開始使用 Python3](#開始使用-python3)
    - [建立虛擬環境](#建立虛擬環境)
    - [進入虛擬環境](#進入虛擬環境)
    - [下載 whl](#下載-whl)
    - [安裝 whl](#安裝-whl)
- [開啟防火牆](#開啟防火牆)
- [建立 django framework](#建立-django-framework)
- [使用 uwsgi](#使用-uwsgi)
  - [app.ini 檔案](#appini-檔案)
- [掛上 nginx](#掛上-nginx)
- [權限修改](#權限修改)

# 加入 nginx repos

此動作再新增下載來源

cd /etc/yum.repos.d/
vi nginx.repo

[Nginx Install document](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)

```
[nginx]
name=nginx repo
baseurl=https://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
```

# 前置作業

## 下載 repos

/var/www/yum-package 是我們的 rpm 檔案放入位置

#yum install -y --downloadonly --downloaddir=/var/www/yum-package/ gcc

#yum install -y --downloadonly --downloaddir=/var/www/yum-package/ python3

#yum install -y --downloadonly --downloaddir=/var/www/yum-package/ python3-devel

#yum install -y --downloadonly --downloaddir=/var/www/yum-package/ redis

#yum install -y --downloadonly --downloaddir=/var/www/yum-package/ nginx


## 下載 sqlite3.rpm

wget https://kojipkgs.fedoraproject.org/packages/sqlite/3.8.3/1.fc21/x86_64/sqlite-devel-3.8.3-1.fc21.x86_64.rpm

wget https://kojipkgs.fedoraproject.org/packages/sqlite/3.8.3/1.fc21/x86_64/sqlite-devel-3.8.3-1.fc21.x86_64.rpm



## 所有的 rpm 檔

<!-- ![rpm package Total](/allrpm.jpg) -->

## 安裝

#表示使用 命令提示字元

### 到 yum-package

#cd /var/www/yum-package/

### 安裝 所有 rpm 檔案

#rpm -ivh *.rpm --nodeps --force


### 查看 sqlite3 版本

#sqlite3 --version

```
3.8.3 2014-02-03 14:04:11 6c643e45c274e755dc5a1a65673df79261c774be
```

OK!!

### 查看有沒有 python3

#python3

```
Python 3.6.8 (default, Nov 16 2020, 16:55:22)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
```

## 開始使用 Python3

### 建立虛擬環境

#python3 -m venv venv

### 進入虛擬環境

#source venv/bin/activate

### 下載 whl

#pip download -d /var/www/pip-package/ django==2.2

#pip download -d /var/www/pip-package/ djangorestframework==3.9.1

#pip download -d /var/www/pip-package/ django-redis==4.10.0

#pip download -d /var/www/pip-package/ uwsgi


### 安裝 whl

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ django

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ djangorestframework

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ django-redis

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ uwsgi

# 開啟防火牆

#firewall-cmd --add-port=80/tcp --permanent

#firewall-cmd --reload

#firewall-cmd --list-all

# 建立 django framework

#cd /var/www

#django-admin startproject demo

#ls

```
demo  pip-package  venv  yum-package
```

#cd demo/

#django-admin startapp demoApp

#python manage.py runserver

#python manage.py migrate

#python manage.py runserver 0.0.0.0:80

這樣會看到黃框框

以下我有使用 vscode SSH 更改 

```/var/www/demo/demo/settings.py``` 裡面的 ```ALLOWED_HOSTS = ['*']```

這樣就可以了

# 使用 uwsgi

/var/www/demo 建立 app.ini 檔

## app.ini 檔案

```
[uwsgi]
uid = root
gid = root
add-gid = awesome
# 替代方案, 如果 sock 無法使用的話 , edu.conf 指向 localhost:9090
http-socket = :9090  
# 主要文件夾
chdir           = /var/www/demo
# projectname.wsgi
module          = demo.wsgi
# 虛擬環境位置
home            = /var/www/venv
master          = true
processes       = 4
# sock 檔的放置位置
socket          = /var/www/demo/app.sock
# sock 檔的權限
chmod-socket    = 666
# 每次會自動清除 sock 檔及執行序
vacuum          = true
```

uwsgi app.ini

```
這邊可以回到防火牆打開 9090 看有沒有出去
```

# 掛上 nginx

cd /etc/nginx/default.d/

vi edu.conf

```
# configuration of the server
server {
    # the port your site will be served on
    listen 81;
    # the domain name it will serve for
    server_name 158.101.144.28;
    # substitute your machine's IP address or FQDN

    index index.html index.htm index.nginx-debian.html;
    location / {
		include /etc/nginx/uwsgi_params;
        root /var/www/demo/;
        uwsgi_pass unix:///var/www/demo/app.sock;
        # uwsgi_params 可以從 nginx 的資料夾複製過來
    }
}
```


# 權限修改

vi /etc/nginx/nginx.conf

```
user nginx 改成 user root
```

vi /etc/selinux/config

```
SELINUX=disabled
```

reboot

vi /var/www/demo/app.ini

```
[uwsgi]
uid = root
gid = root
```

