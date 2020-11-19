# django_uwsgi_Offline

yum install -y python3

vi /etc/yum.repos.d/nginx.repo

```
[nginx]
name=nginx repo
baseurl=https://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
```

yum install -y nginx

----------------------

mkdir /var/www
cd /var/www

python3 -m venv venv 
source venv/bin/activate

(venv)# pip install django==2.2
(venv)# pip install uwsgi

### Error: Exception: you need a C compiler to build uWSGI

### 退出虛擬環境
deactivate

yum install -y gcc

source venv/bin/activate
(venv)# pip install uwsgi

### Error #include <Python.h> compilation

### 退出虛擬環境
deactivate

yum install -y python3-devel

source venv/bin/activate
(venv)# pip install uwsgi

OK !!

yum install wget

source venv/bin/activate
(venv)# pip install uwsgi

./configure --prefix=$HOME/opt/sqlite


＃更新SQLite 3
＃获取源代码（在主目录中运行）
[root@djangoServer ~]# cd ~
[root@djangoServer ~]# wget https://www.sqlite.org/2019/sqlite-autoconf-3270200.tar.gz
[root@djangoServer ~]# tar -zxvf sqlite-autoconf-3270200.tar.gz

＃构建并安装
[root@djangoServer ~]# cd sqlite-autoconf-3270200
[root@djangoServer sqlite-autoconf-3270200]# ./configure --prefix=/usr/local
[root@djangoServer sqlite-autoconf-3270200]# make && make install
[root@djangoServer sqlite-autoconf-3270200]# find /usr/ -name sqlite3
/usr/bin/sqlite3
/usr/lib64/python2.7/sqlite3
/usr/local/bin/sqlite3
/usr/local/python3/lib/python3.7/site-packages/django/db/backends/sqlite3
/usr/local/python3/lib/python3.7/sqlite3
[root@djangoServer sqlite-autoconf-3270200]# 

＃不必要的文件，目录删除
[root@djangoServer sqlite-autoconf-3270200]# cd ~
[root@djangoServer ~]# ls
anaconda-ks.cfg  sqlite-autoconf-3270200  sqlite-autoconf-3270200.tar.gz
[root@djangoServer ~]# 
[root@djangoServer ~]# rm -rf sqlite-autoconf-3270200.tar.gz
[root@djangoServer ~]# rm -rf sqlite-autoconf-3270200

＃检查版本
## 最新安装的sqlite3版本
[root@djangoServer ~]# /usr/local/bin/sqlite3 --version
3.27.2 2019-02-25 16:06:06 bd49a8271d650fa89e446b42e513b595a717b9212c91dd384aab871fc1d0f6d7
[root@djangoServer ~]# 

## Centos7自带的sqlite3版本
[root@djangoServer ~]# /usr/bin/sqlite3 --version
3.7.17 2013-05-20 00:56:22 118a3b35693b134d56ebd780123b7fd6f1497668
[root@djangoServer ~]# 

## 可以看到sqlite3的版本还是旧版本，那么需要更新一下。
[root@djangoServer ~]# sqlite3 --version
3.7.17 2013-05-20 00:56:22 118a3b35693b134d56ebd780123b7fd6f1497668
[root@djangoServer ~]# 

## 更改旧的sqlite3
[root@djangoServer ~]# mv /usr/bin/sqlite3  /usr/bin/sqlite3_old

## 软链接将新的sqlite3设置到/usr/bin目录下
[root@djangoServer ~]# ln -s /usr/local/bin/sqlite3   /usr/bin/sqlite3

## 查看当前全局sqlite3的版本
[root@djangoServer ~]# sqlite3 --version
3.27.2 2019-02-25 16:06:06 bd49a8271d650fa89e446b42e513b595a717b9212c91dd384aab871fc1d0f6d7
[root@djangoServer ~]# 

sudo su

systemctl status nginx
systemctl start nginx

firewall-cmd --add-port=80/tcp --permanent
firewall-cmd --reload
firewall-cmd --list-all

#nginx conf

