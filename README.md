#Nginx Configs

Collection of Nginx configs for most popular CMS/CMF/Frameworks based on PHP.

##Table of Contents

- [Asgard CMS](#asgard-cms)
- [Bolt CMS](#bolt-cms)
- [CMS Made Simple](#cms-made-simple)
- [Codeigniter](#codeigniter)
- [Data Life Engine](#data-life-engine)
- [Drupal 7, 8](#drupal-7-8)
- [FuelPHP](#fuelphp)
- [Joomla 2, 3](#joomla-2-3)
- [KodiCMS](#kodicms)
- [Kohana](#kohana)
- [Laravel](#laravel)
- [MaxSite CMS](#maxsite-cms)
- [MediaWiki](#mediawiki)
- [MODx Revolution](#modx-revolution)
- [Octobercms](#octobercms)
- [OpenCart 1.5](#opencart-15)
- [phpBB3](#phpbb3)
- [ProcessWire 2](#processwire-2)
- [Symfony](#symfony)
- [Wordpress 4](#wordpress-4)
- [Yii Advanced](#yii-advanced)
- [Yii Basic](#yii-basic)
- [ZenCart 1.5](#zencart-15)
- [Zend Framework](#zend-framework)


###Asgard CMS

```
root /home/u1/domains/example.com/public;

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location ~ (^|/)\. {
  return 403;
}

location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location /vendor/ {
  location ~ .*\.(js|css|png|jpg|jpeg|gif|ico)?$ {
    root /home/u1/domains/example.com;
  }
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

```

###Bolt CMS

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location ~* /thumbs/(.*)$ {
  try_files $uri $uri/ /app/classes/timthumb.php?$query_string;
}

location /app/classes/upload {
  try_files $uri $uri/ /app/classes/upload/index.php?$query_string;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

location ~ /\. {
  deny all;
}

location /app {
  deny all;
}

location ~ cache/ {
  return 403;
}

location ~ /vendor {
  deny all;
}

location ~ \.db$ {
  deny all;
}

location ~*  \.(jpg|jpeg|png|gif|ico|css|js)$ {
  expires 7d;
}

```

###CMS Made Simple

```
root /home/u1/domains/example.com;

location ~ /\. {
  deny all;
}

location / {
  try_files $uri $uri/ @cmsmadesimple;
}

location @cmsmadesimple {
  rewrite  ^\/?(.*)\.html$  /index.php?page=$1  last;
  rewrite  ^\/?(.*)$    /index.php?page=$1  last;
  break;
}

location ~ \.php$ {
  include fastcgi_params;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_index index.php;
  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
}
```

###Codeigniter

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ /index.php;
}

location ~* \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

location ~*  \.(jpg|jpeg|png|gif|ico|css|js)$ {
  expires 7d;
}

```

###Concerte5

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ /index.php$request_uri /index.php;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  try_files $uri =404;
}

location ~ /\. {
  deny all;
  access_log off;
  log_not_found off;
}

location ~*  \.(jpg|jpeg|png|gif|ico|css|js)$ {
  expires 7d;
}

```

###Data Life Engine

```
root /home/u1/domains/example.com;

location / {
  rewrite ^/page/(.*)$ /index.php?cstart=$1;
  
  rewrite ^/([0-9]+)/([0-9]+)/([0-9]+)/page,([0-9]+),([0-9]+),(.*).html*$ /index.php?subaction=showfull&year=$1&month=$2&day=$3&news_page=$4&cstart=$5&news_name=$6;
  rewrite ^/([0-9]+)/([0-9]+)/([0-9]+)/page,([0-9]+),(.*).html*$ /index.php?subaction=showfull&year=$1&month=$2&day=$3&news_page=$4&news_name=$5;
  rewrite ^/([0-9]+)/([0-9]+)/([0-9]+)/print:page,([0-9]+),(.*).html*$ /engine/print.php?subaction=showfull&year=$1&month=$2&day=$3&news_page=$4&news_name=$5;
  rewrite ^/([0-9]+)/([0-9]+)/([0-9]+)/(.*).html*$ /index.php?subaction=showfull&year=$1&month=$2&day=$3&news_name=$4;
  
  rewrite ^/([^.]+)/page,([0-9]+),([0-9]+),([0-9]+)-(.*).html(/?)$ /index.php?newsid=$4&news_page=$2&cstart=$3;
  rewrite ^/([^.]+)/page,([0-9]+),([0-9]+)-(.*).html(/?)$ /index.php?newsid=$3&news_page=$2;
  rewrite ^/([^.]+)/print:page,([0-9]+),([0-9]+)-(.*).html(/?)$ /engine/print.php?news_page=$2&newsid=$3;
  rewrite ^/([^.]+)/([0-9]+)-(.*).html(/?)$ /index.php?newsid=$2;
  
  rewrite ^/page,([0-9]+),([0-9]+),([0-9]+)-(.*).html(/?)+$ /index.php?newsid=$3&news_page=$1&cstart=$2;
  rewrite ^/page,([0-9]+),([0-9]+)-(.*).html(/?)+$ /index.php?newsid=$2&news_page=$1;
  rewrite ^/print:page,([0-9]+),([0-9]+)-(.*).html(/?)+$ /engine/print.php?news_page=$1&newsid=$2;
  rewrite ^/([0-9]+)-(.*).html(/?)+$ /index.php?newsid=$1;
  
  rewrite ^/([0-9]+)/([0-9]+)/([0-9]+)/page/([0-9]+)(.*)$ /index.php?year=$1&month=$2&day=$3&cstart=$4;
  rewrite ^/([0-9]+)/([0-9]+)/([0-9]+)(.*)$ /index.php?year=$1&month=$2&day=$3;
  
  rewrite ^/([0-9]+)/([0-9]+)/page/([0-9]+)(.*)$ /index.php?year=$1&month=$2&cstart=$3;
  rewrite ^/([0-9]+)/([0-9]+)(.*)$ /index.php?year=$1&month=$2;
  
  rewrite ^/([0-9]+)/page/([0-9]+)(.*)$ /index.php?year=$1&cstart=$2;
  rewrite ^/([0-9]+)(.*)$ /index.php?year=$1;
  
  rewrite ^/catalog/([^/]*)/page/([0-9]+)(.*)$ /index.php?catalog=$1&cstart=$2;
  rewrite ^/catalog/([^/]*)(.*)$ /index.php?catalog=$1;
  
  rewrite ^/newposts/page/([0-9]+)(.*)$ /index.php?subaction=newposts&cstart=$1;
  rewrite ^/newposts(.*)$ /index.php?subaction=newposts;
  
  rewrite ^/static/(.*).html(.*)$ /index.php?do=static&page=$1;
  
  rewrite ^/user/([^/]*)/news/page/([0-9]+)(.*)$ /index.php?subaction=allnews&user=$1&cstart=$2;
  rewrite ^/user/([^/]*)/news(.*)$ /index.php?subaction=allnews&user=$1;
  rewrite ^/user/([^/]*)(.*)$ /index.php?subaction=userinfo&user=$1;
  rewrite ^/favorites /index.php?do=favorites;
  rewrite ^/favorites/page/(.*)$ /index.php?do=favorites&cstart=$1;
  rewrite ^/statistics.html$ /index.php?do=stats;
  rewrite ^/addnews.html(.*)$ /index.php?do=addnews;
  rewrite ^/rss.xml$ /engine/rss.php;
  rewrite ^/sitemap.xml$ uploads/sitemap.xml;
  
  if (!-d $request_filename) {
    rewrite ^/([^.]+)/page/([0-9]+)(.*)$ /index.php?do=cat&category=$1&cstart=$2;
    rewrite ^/([^.]+)/*$ /index.php?do=cat&category=$1;       
  }
  
  if (!-f $request_filename) {
    rewrite ^/([^<]+)/rss.xml$ /engine/rss.php?do=cat&category=$1;
    rewrite ^/page,([0-9]+),([^/]+).html$ /index.php?do=static&page=$2&news_page=$1;
    rewrite ^/([^/]+).html$ /index.php?do=static&page=$1;
  }
}

location /xfsearch {
  rewrite ^/xfsearch/([^/]*)/*$ /index.php?do=xfsearch&xf=$1;
  rewrite ^/xfsearch/([^/]*)/page/([0-9]+)/*$ /index.php?do=xfsearch&xf=$1&cstart=$2;
}

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
  try_files $uri =404;
}

location ~* (engine/cache) {
  deny all;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

```

###Drupal 7, 8

```
root /home/u1/domains/example.com;

location = /rss.xml {
  rewrite ^ /index.php?q=rss.xml;
}

location = /sitemap.xml {
  try_files $uri /index.php?q=sitemap.xml;
}

location / {
  location ^~ /system/files/ {
    fastcgi_pass unix:/var/run/php5-example.com.sock;
    log_not_found off;
  }
  
  location ^~ /sites/default/files/private/ {
    internal;
  }
  
  location ^~ /system/files_force/ {
    fastcgi_pass unix:/var/run/php5-example.com.sock;
    log_not_found off;
  }
  
  location ~* /imagecache/ {
    access_log off;
    expires 30d;
    try_files $uri @drupal;
  }
  
  location ~* /files/styles/ {
    access_log off;
    expires 30d;
    try_files $uri @drupal;
  }
  
  location ~* ^.+\.(?:css|cur|js|jpe?g|gif|htc|ico|png|html|xml|otf|ttf|eot|woff|svg)$ {
    access_log off;
    expires 30d;
    tcp_nodelay off;
  }
  
  location ~* ^.+\.(?:pdf|pptx?)$ {
    expires 30d;
    tcp_nodelay off;
  }
  
  location ^~ /help/ {
    location ~* ^/help/[^/]*/README\.txt$ {
      fastcgi_pass  unix:/var/run/php5-example.com.sock;
    }
  }
  
  location ~* ^(?:.+\.(?:htaccess|make|engine|inc|info|install|module|profile|po|pot|sh|.*sql|test|theme|tpl(?:\.php)?|xtmpl)|code-style\.pl|/Entries.*|/Repository|/Root|/Tag|/Template)$ {
    return 404;
  }
  
  try_files $uri $uri/ @cache;
}

location @cache {
  if ($query_string ~ ".+") {
    return 405;
  }
  
  if ($http_cookie ~ "DRUPAL_UID" ) {
    return 405;
  } 
  
  if ($request_method !~ ^(GET|HEAD)$ ) {
    return 405;
  }
  
  error_page 405 = @drupal;
  expires 12h;
  add_header Cache-Control "no-store, no-cache, must-revalidate, post-check=0, pre-check=0";
  try_files /cache/normal/$host/${uri}_.html /cache/perm/$host/${uri}_.css /cache/perm/$host/${uri}_.js /cache/$host/0$uri.html /cache/$host/0${uri}/index.html @drupal;
}

location @drupal {
  rewrite ^ /index.php;
}

location @drupal-no-args {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_intercept_errors on;
  fastcgi_pass  unix:/var/run/php5-example.com.sock;
}

location ~ /\. {
  deny all;
}

location @empty {
  expires 30d;
}

location = /boost_stats.php {
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass  unix:/var/run/php5-example.com.sock;
}

```
###FuelPHP

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ @fuel;
  expires 30d;
}

location @fuel {
  rewrite ^ /index.php?/$request_uri;
}

location ~* ^/(modules|application|system) {
  return 403;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

location ~*  \.(jpg|jpeg|png|gif|ico|css|js)$ {
  expires 7d;
}

```

###Joomla 2, 3

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ /index.php?$args;
}

location ~* /(images|cache|media|logs|tmp)/.*\.(php|pl|py|jsp|asp|sh|cgi)$ {
  return 403;
}

location ~ \.php$ {
  include         fastcgi_params;
  fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

location ~* ^.+\.(jpg|jpeg|gif|css|png|js|ico|bmp)$ {
  access_log       off;
  expires          10d;
  break;
}

location ~ /\. {
  deny all;
}

```

###KodiCMS

```
root /home/u1/domains/example.com/public;

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

location ~*  \.(jpg|jpeg|png|gif|ico|css|js)$ {
  expires 7d;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location ~ (^|/)\. {
  return 403;
}

location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location /vendor/ {
  location ~ .*\.(js|css|png|jpg|jpeg|gif|ico)?$ {
    root /home/u1/domains/example.com;
  }
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

```

###Kohana

```
root /home/u1/domains/example.com;

location /
{
    try_files $uri /index.php?$args;

    if (!-e $request_filename)
    {
        rewrite ^/(.*)$ /index.php/$1 last;
    }
}

location ~ /\.
{
    deny  all;
}

location ~ \.php$
{
    try_files $uri = 404;
    fastcgi_pass unix:/var/run/php5-example.com.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}

location ~ /\. {
  deny all;
}

```

###Laravel

```
root /home/u1/domains/example.com/public;

location ~ /\. {
  deny all;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location ~ (^|/)\. {
  return 403;
}

location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location /vendor/ {
  location ~ .*\.(js|css|png|jpg|jpeg|gif|ico)?$ {
    root /home/u1/domains/example.com;
  }
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}
```

###MaxSite CMS

```
root /home/u1/domains/example.com;

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location ~ (^|/)\. {
  return 403;
}

location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

```

###MediaWiki

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ @rewrite;
}

location @rewrite {
  rewrite ^/(.*)$ /index.php?title=$1;
}

location ^~ /maintenance/ {
  return 403;
}

location ~ \.php$ {
  fastcgi_param PATH_INFO $path_info;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_index index.php;
  include fastcgi.conf;
}

location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
  try_files $uri /index.php;
  expires max;
  log_not_found off;
}

location = /_.gif {
  expires max;
}

location ^~ /cache/ {
  deny all;
}

location /dumps {
  root /home/u1/domains/example.com/local;
  autoindex on;
}

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}
```

###MODx Revolution

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ @rewrite;
}

location /core {
  deny all;
  return 403;
}

location @rewrite {
  rewrite ^/(.*)$ /index.php?q=$1;
}


location ~ \.php$ {
  try_files $uri =404;
  include fastcgi_params;
  fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

```

###Octobercms

```
root /home/u1/domains/example.com;

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location ~ (^|/)\. {
  return 403;
}

location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

```

###OpenCart 1.5

```
root /home/u1/domains/example.com;

rewrite /vqmod/install$ $scheme://$host$uri/ permanent;

location /vqmod/install/ {
  index index.php;
}

location = /sitemap.xml {
  rewrite ^(.*)$ /index.php?route=feed/google_sitemap break;
}

location = /googlebase.xml {
  rewrite ^(.*)$ /index.php?route=feed/google_base break;
}

location / {
  try_files $uri $uri/ @opencart;
}

location @opencart {
  rewrite ^/(.+)$ /index.php?_route_=$1 last;
}

location ~* \.(engine|inc|info|ini|install|log|make|module|profile|test|po|sh|.*sql|theme|tpl(\.php)?|xtmpl)$|^(\..*|Entries.*|Repository|Root|Tag|Template)$|\.php_ {
  deny all;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /apple-touch-icon.png {
  log_not_found off;
  access_log off;
}

location = /apple-touch-icon-precomposed.png {
  log_not_found off;
  access_log off;
}

location ~* \.(?:3gp|gif|jpg|jpe?g|png|ico|wmv|avi|asf|asx|mpg|mpeg|mp4|pls|mp3|mid|wav|swf|flv|txt|js|css|exe|zip|tar|rar|gz|tgz|bz2|uha|7z|doc|docx|xls|xlsx|pdf|iso|woff)$ {
  expires max;
  add_header Pragma public;
  add_header Cache-Control "public, must-revalidate, proxy-revalidate";
}


location ~ ~$ {
  access_log off;
  log_not_found off;
  deny all;
}

location ~* /(?:cache|logs|image|download)/.*\.php$ {
  deny all;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location ~* \.(eot|otf|ttf|woff)$ {
  add_header Access-Control-Allow-Origin *;
}

location ~ [^/]\.php(/|$) {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  try_files $fastcgi_script_name =404;
  set $path_info $fastcgi_path_info;
  fastcgi_param PATH_INFO $path_info;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_index index.php;
  include fastcgi.conf;
}
```

###phpBB3

```
root /home/u1/domains/example.com;

location / {
  index index.php index.html index.htm;
}


location ~ /(config\.php|common\.php|includes|cache|files|store|images/avatars/upload) {
  deny all;
  internal;
}


location ~ \.php$ {
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
  include fastcgi_params;
}

location ~ /\. {
  deny all;
}

```

###ProcessWire 2

```
root /home/u1/domains/example.com;

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location / {
  try_files $uri $uri/ /index.php?it=$uri&$args;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  try_files $uri =404;
}

location ~ /\. {
  deny all;
}

location ~ /(COPYRIGHT|LICENSE|README|htaccess)\.txt {
  deny  all;
}

location ~ ^/site(-[^/]+)?/assets/(.*\.php|backups|cache|config|install|logs|sessions) {
  deny  all;
}

location ~ ^/site(-[^/]+)?/install {
  deny  all;
}

location ~ ^/(site(-[^/]+)?|wire)/(config(-dev)?|index\.config)\.php {
  deny  all;
}

location ~ ^/((site(-[^/]+)?|wire)/modules|wire/core)/.*\.(inc|module|php|tpl) {
  deny  all;
}

location ~ ^/(site(-[^/]+)?|wire)/templates(-admin)?/.*\.(inc|html?|php|tpl) {
  deny  all;
}

location ~*  \.(jpg|jpeg|png|gif|ico|css|js)$ {
  expires 7d;
}

```

###Symfony

```
root /home/u1/domains/example.com/web;

location / {
  try_files $uri /app.php$is_args$args;
}

location ~ ^/(app_dev|config)\.php(/|$) {
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_split_path_info ^(.+\.php)(/.*)$;
  include fastcgi_params;
  fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
}

location ~ ^/app\.php(/|$) {
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_split_path_info ^(.+\.php)(/.*)$;
  include fastcgi_params;
  fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
  internal;
}

location ~ /\. {
  deny all;
}
```

###Wordpress 4

```
root /home/u1/domains/example.com;

location ~ /\. {
  deny all;
}

location = /favicon.ico {
  log_not_found off;
  access_log off;
}

location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}

location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location ~ \.php$ {
  include fastcgi.conf;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
  expires max;
  log_not_found off;
}
```

###Yii Advanced

```
root /home/u1/domains/example.com/frontend/web;

location /backend {
  alias /home/u1/domains/example.com/backend/web;
}

location / {
  try_files $uri $uri/ /index.php?$args;
}

location ~ ^/(protected|framework|themes/\w+/views) {
  deny  all;
}

location ~ /\. {
  deny all;
}

location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
  try_files $uri =404;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

```

###Yii Basic

```
root /home/u1/domains/example.com/web;

location / {
  try_files $uri $uri/ /index.php?$args;
}

location ~ ^/(protected|framework|themes/\w+/views) {
  deny  all;
}

location ~ /\. {
  deny all;
}

location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
  try_files $uri =404;
}

location ~ \.php$ {
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $request_filename;
  fastcgi_intercept_errors on;
  fastcgi_pass unix:/var/run/php5-example.com.sock;
}

```

###ZenCart 1.5

```
root /home/u1/domains/example.com;

location / {
  try_files $uri $uri/ index.php;
}

location /docs {
  if ($request_uri ~*
  (^\/|\.js|\.css|\.jpg|\.gif|\.png|\.html)$ ) {
    break;
  }
  return 403;
}

location /editors {
  if ($request_uri ~*
  (^\/|\.js|\.css|\.jpg|\.gif|\.png|\.html|\.xml)$ ) {
    break;
  }
  return 403;
}

location /email {
  if ($request_uri ~*
  (^\/|\.jpg|\.JPG|\.jpeg|\.JPEG|\.gif|\.GIF|\.png|\.PNG)$ ) {
    break;
  }
  return 403;
}

location /extras {
  if ($request_uri ~* (^\/|\.php|\.html)$ ) {
    break;
  }
  return 403;
}

location /images {
  if ($request_uri ~*
  (^\/|\.jpg|\.JPG|\.jpeg|\.JPEG|\.gif|\.GIF|\.png|\.PNG|\.swf|\.SWF|\.WMA)$
  ) {
    break;
  }
  return 403;
}

location /(download|pub) {
  if ($request_uri ~*
  (^\/|\.zip|\.ZIP|\.gzip|\.pdf|\.PDF|\.mp3|\.MP3|\.swf|\.SWF|\.wma|\.WMA|\.wmv|\.WMV)$
  ) {
    break;
  }
  return 403;
}

location /includes {
  if ($request_uri ~*
  (^\/|\.js|\.JS|\.css|\.CSS|\.jpg|\.JPG|\.gif|\.GIF|\.png|\.PNG|\.swf|\.SWF|\.xsl|\.XSL)$
  ) {
    break;
  }
  return 403;
}

location /media {
  if ($request_uri ~*
  (^\/|\.mp3|\.mp4|\.swf|\.avi|\.mpg|\.wma|\.rm|\.ra|\.ram|\.wmv)$ ) {
    break;
  }
  return 403;
}

location /admin {
  if ($request_uri ~*
  (^\/|\.php|\.js|\.css|\.jpg|\.gif|\.png)$ ) {
    break;
  }
  return 403;
}

location = /robots.txt  {
  access_log off;
  log_not_found off;
}

location = /favicon.ico {
  access_log off;
  error_log off;
  log_not_found off;
}

location ~ /\. {
  deny all;
}

location ~* \.php$ {
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_index index.php;
  fastcgi_split_path_info ^(.+\.php)(.*)$;
  include fastcgi_params;
  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
}
```

###Zend Framework

```
root /home/u1/domains/example.com/public;

location / {
  try_files $uri $uri/ /index.php$is_args$args;
}

location ~ \.php$ {
  fastcgi_pass unix:/var/run/php5-example.com.sock;
  fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
  include        fastcgi_params;
}

location ~ /\.svn {
  deny all;
}

location ~ /\.git {
  deny all;
}

location ~ /\.hg {
  deny all;
}
```
