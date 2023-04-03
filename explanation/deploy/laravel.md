## LaravelのEC2デプロイ手順  
### 各種インストール
1.パッケージのアップデート
```
sudo yum update
```
2.NGINXをインストール
```
sudo amazon-linux-extras install nginx1
```
3.サーバーを起動
```
sudo systemctl start nginx
```
4.起動確認
```
sudo systemctl status nginx
```
5.再起動時の実行設定(NGINX)
```
sudo systemctl enable nginx
```
6.PHPをインストール
```
sudo amazon-linux-extras install php8.0
```
7.fmp起動
```
sudo systemctl start php-fpm.service
```
8.再起動時の実行設定(php-fpm)
```
sudo systemctl enable nginx
```
9.Composerをインストール
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```
10.パスを通す
```
sudo mv composer.phar /usr/local/bin/composer
```
11.Gitをインストール
```
sudo yum install git
```
### 設定変更
1.php-fpm
・移動
```
 sudo vi /etc/php-fpm.d/www.conf
```
・.confを修正
```
user = nginx
group = nginx

listen = /var/run/php-fpm/php-fpm.sock

listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```
・再起動
```
sudo systemctl restart php-fpm.service
```
2.NGINX
・移動
```
sudo vi /etc/nginx/nginx.conf
```
・修正
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen 80;
        server_name _;
        root /var/www/public;

        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Content-Type-Options "nosniff";

        index index.php;

        charset utf-8;

        location / {
            try_files $uri $uri/ /index.php?$query_string;
        }

        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt  { access_log off; log_not_found off; }

        error_page 404 /index.php;

        location ~ \.php$ {
            fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            include fastcgi_params;
        }

        location ~ /\.(?!well-known).* {
            deny all;
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2;
#        listen       [::]:443 ssl http2;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers PROFILE=SYSTEM;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}
```
再起動
```
sudo nginx -t
```
```
sudo systemctl restart nginx
```
```
sudo systemctl reload nginx
```
3.rootの権限変更
```
sudo mkdir /var/www
```
```
sudo chown ec2-user:nginx /var/www
```
```
udo chmod 2775 /var/www
```
```
sudo usermod -a -G nginx ec2-user
```
### GitHubとSSHの設定
1.SSHキーの作成
```
cd ~/.ssh
```
```
ssh-keygen
```
```
// コピーしてGitHubに登録
sudo cat id_rsa.pub
```
2.接続確認
```
ssh -T git@github.com
```
3.リポジトリをclone
```
cd /var/www
```
```
git clone [リポジトリURL] .
```
