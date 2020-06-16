
# Docker - MongoDB

- .env file을 사용해서 컨테이너에 정보를 주도록 해보겠습

## docker-compose.yml  작성

```yaml
version: '3'
services:
    mongodb:
        image: mongo
        ports:
            - "${MONGO_PORT}:27017"
        volumes:
            - /Users/wool/Dev/mongodb:/data/db
        container_name: "docker-mongodb"
        env_file:
            - .env
```

## .env file 작성

```
## Mongodb
MONGO_HOST=localhost
MONGO_PORT=27017
MONGO_INITDB_ROOT_USERNAME=root
MONGO_INITDB_ROOT_PASSWORD=qwerqwer123
MONGO_INITDB_DATABASE=mongo-test
```

## docker-compose up

```bash
$ docker-compose up -d
```

## Add user (db.createUser)

.env 파일에 적용했던 username, password를 사용해서 admin에 접근 한 후, 사용자를 만들어주도록 하겠습니다.

### mongodb shell 접근

```bash
$ docker exec -it docker-mongodb bash

root@33045c9dba7a:$ mongo
MongoDB shell version v4.2.7
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("96f78619-6b89-48f5-a860-2c90e554607c") }
MongoDB server version: 4.2.7
```

### db.auth()로 admin 인증하기

```bash
> use admin
> db.auth('root','qwerqwer123') # .env에 썼었던 계정으로 접속 한 후에 진행
1
```

위처럼 1이 나오면 인증이 완료 된 것입니다

- tips

    바로 인증하고 접속하기 

    ```bash
    > mongo -u username -p password —authenticationDatabase admin
    ```

    바로 인증하고 admin 데이터베이스로 접속하기

    ```bash
    > mongo admin -u username -p password —authenticationDatabase admin
    ```

### 데이터베이스에 접근

```bash
> use test_db
```

'use' 뒤에 db이름을 적어주면 해당 db로 접근이 됩니다

### 사용자 만들기

```bash
> db.createUser({
	user:'wool',
	pwd:'password',
	roles:['dbOwner']
})
```

사용자의 roles는 공식 doc를 참고했다

[Built-In Roles - MongoDB Manual](https://docs.mongodb.com/manual/reference/built-in-roles/)