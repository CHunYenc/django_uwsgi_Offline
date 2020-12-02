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
  - [暫時性的關掉或開啟 selinux](#暫時性的關掉或開啟-selinux)
  - [永久性的關掉 selinux](#永久性的關掉-selinux)
  - [尚未測試](#尚未測試)
- [uwsgi 自動啟動](#uwsgi-自動啟動)
  - [創建 uwsgi.service File](#創建-uwsgiservice-file)

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

#yum install -y --downloadonly --downloaddir=/var/www/yum-package/ 
epel-release
gcc
gcc-c++
python3
python3-devel
redis
nginx
yum-utils
clang
htop
screen

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

#pip download -d /var/www/pip-package/ requests==2.21.0

#pip download -d /var/www/pip-package/ pandas==0.23.4

#pip download -d /var/www/pip-package/ simplejson==3.17.2

#pip download -d /var/www/pip-package/ PyMySQL==0.9.3

#pip download -d /var/www/pip-package/ redis==3.3.11

#pip download -d /var/www/pip-package/ PuLP==2.3.1

#pip download -d /var/www/pip-package/ django-cors-headers==3.4.0

#pip download -d /var/www/pip-package/ plotly==4.6.0

#pip download -d /var/www/pip-package/ matplotlib==3.0.2

pip download -d /var/www/pip-package/ llvmlite==0.31.0

#pip download -d /var/www/pip-package/ numba==0.41.0

#pip download -d /var/www/pip-package/ uwsgi


### 安裝 whl

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ django

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ djangorestframework

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ requests

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ pandas

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ simplejson

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ PyMySQL

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ redis

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ PuLP

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ django-cors-headers

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ plotly

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ matplotlib

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ llvmlite

#pip install --use-wheel --no-index --find-links=/var/www/pip-package/ numba

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

# 替代方案, 如果 sock 無法使用的話 , edu.conf 指向 localhost:9090
http-socket = :9090
# 主要文件夾

chdir           = /var/www/project
# project.wsgi
module          = project.wsgi
# 虛擬環境位置
home            = /var/www/project/venv
master          = true
enable-threads  = true
single-interpreter = true
processes = 4
threads = 2
# sock 檔的放置位置
socket          = /var/www/project/app.sock
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

vi frontend.conf

```
# configuration of the server
server {
    # the port your site will be served on
    listen 80;
    # the domain name it will serve for
    server_name 140.114.60.61;
    # substitute your machine's IP address or FQDN

    location / {
	try_files $uri $uri/ /index.html;
        root   /var/www/dist/;
        index  index.html index.htm;
    }
}
```

vi backend.conf

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

source_1 : [黃昏的甘蔗](https://blog.xuite.net/tolarku/blog/195633562-CentOS+%E9%97%9C%E9%96%89+selinux)

source_2 : [爆走程式碼](https://dotblogs.com.tw/echo/2017/06/19/linux_selinux_mode)

## 暫時性的關掉或開啟 selinux

$ getenforce
Enforcing
$ sudo setenforce 0
$ getenforce
Permissive
$ sudo setenforce 1
$ getenforce
Enforcing

 

## 永久性的關掉 selinux 

$ sudo vi /etc/sysconfig/selinux     


``` SELINUX=enforcing ``` 更改為 ```SELINUX=disabled```


更改完後 ```reboot``` 或 ```restart```


## 尚未測試 

#vi /etc/nginx/nginx.conf

```
user nginx 改成 user root
```

vi /var/www/demo/app.ini

```
[uwsgi]
uid = root
gid = root
```

# uwsgi 自動啟動

## 創建 uwsgi.service File 

#vi /etc/systemd/system/uwsgi.service

