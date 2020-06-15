# Docker - Mysql
# Docker-db

Docker를 사용해서 db를 구축 하자.
Docker를 사용하여 DB를 구축하는 과정이기 때문에, Docker, Docker-compose는 이미 설치되어있다는 가정 하에 작성


## docker-compose.yml 작성

```bash
version: "3"
services:
	db:
		images: mysql:latest
		container_name: docker-mysql
		ports:
			- "3306:3306"
		environment:
			MYSQL_ROOT_PASSWORD: 'qwerqwer123'
		command:
			- --character-set-server=utf8mb4
			- --collation-server=utf8mb4_unicode_ci
		volumes:
			- /Users/wool/Dev/db_data_docker:/var/lib/mysql
```

- container 이름 : docker-mysql
- mysql db port : 3306
- root password : qwerqwer123

## docker-compose up

```bash
$ (yml파일경로)docker-compose up -d
```

## docker db create user

docker의 bash로 들어가서 mysql을 설정 해준다

```bash
$ docker exec -it docker-mysql bash
```

mysql 루트계정 접속

```bash
$ mysql -u root -p

Enter password: qwerqwer123
```

mysql 계정 생성

```bash
mysql> CREATE USER 'wool'@'%' IDENTIFIED BY 'qwerqwer123';

mysql> GRANT ALL PRIVILEGES ON *.* TO 'wool'@'%';

mysql> flush privileges;
```

이제 생성한 wool계정으로 접속하면 된다.
