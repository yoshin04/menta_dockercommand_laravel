1. リポジトリをクローンする
```
git clone <リポジトリ名>
```

2. クローンしたリポジトリに移動する
```
cd <リポジトリ>
```

3. phpディレクトリのdocker image作成
```
docker image build -t menta-php ./docker/php
```

4. phpコンテナの起動
```
docker run --name menta-php -v /Users/yoshidataiga/menta_dockercommand_laravel/src:/src -d menta-php
```

5. phpコンテナの中に入ってIPアドレスを調べる
```
docker exec -it menta-php bash
hostname -i
exit
```

6. nginxディレクトリのdefault.confの下記の部分を調べたIPアドレスに書き換える
```
fastcgi_pass <調べたIPアドレス>:9000;
```

7. nginxディレクトリのdocker imageを作成
```
docker image build -t menta-nginx ./docker/nginx
```

8. nginxコンテナの起動  
```
docker run --name menta-nginx -v /Users/yoshidataiga/menta_dockercommand_laravel/src:/src -p 80:80 -d menta-nginx
```

9. dockerボリュームの作成
```
docker volume create db_storage
```

10. mysqlコンテナの起動  
```
docker run -d --name menta-db -e MYSQL_DATABASE=db_name -e MYSQL_ROOT_PASSWORD=root -v db_storage:/var/lib/mysql -p 3306:3306 mysql:5.7 
```

. mysqlコンテナの中に入りIPアドレスを調べる
```
docker exec -it menta-db bash
```

11. mysqlコンテナのIPアドレスを調べる
```
docker exec -it <mysqlコンテナ> bash
hostname -i 
exit
```

12. PHPコンテナに入ってLaravelの.envを編集する
```
docker exec -it <PHPコンテナ> bash
vi .env
DB_CONNECTION=mysql  
DB_HOST=<mysqlコンテナのIPアドレス>  
DB_PORT=3306  
DB_DATABASE=db_name  
DB_USERNAME=root  
DB_PASSWORD=root
```  



