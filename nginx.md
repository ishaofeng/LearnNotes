##nginx服务器配置
***
###1.安装
ubuntu:
    apt-get install nginx-full nginx-common
    apt-get install uwsgi uwsgi-plugin-python
location / {
    include uwsgi_params;
    uwsgi_pass unix:/tmp/uwsgi.myapp.socket;
    uwsgi_param UWSGI_PYHOME /var/www/myapp/env;
    uwsgi_param UWSGI_CHIDIR /var/www/myapp
    uwsgi_param UWSGI_SCRIPT index; #这个是执行的app主文件
}
