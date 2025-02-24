### Система электронного документооборота

### Запуск
- cd docker, docker compose -f docker-compose-db.yaml up -d для поднятия базы, минио. в общем пакете лежит сборщик бекэнд+бд.

### Запуск на сервере.
Автоматический апдейт после мержа в мейн, если нужно вручную перезапустить то. 
- cd docker. docker compose -f docker-compose-server.yaml up -d

### Схема сущностей приложения 
![schemeBD.PNG](img%2FschemeBD.PNG)
### Логика статусов документов
![docStatus.PNG](img%2FdocStatus.PNG)


### Swagger
http://localhost:8080/api/v1/swagger-ui/index.html

#### Поднять приложение
#### Из папки на сервере можно поднять компоуз
```
сd docker 

sudo docker compose -f docker-compose-server.yaml up -d
```

#### Отдельно каждый образ (not actual)
```
sudo docker run -d --net=host --restart unless-stopped \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=postgres \
-e POSTGRES_DB=edm \
moongrail/caselab-edm-db:0.0.1

sudo docker run -d --name minio \
--restart unless-stopped --net=host \
-e MINIO_ROOT_USER=minio-user \
-e MINIO_ROOT_PASSWORD=minio-password \
moongrail/caselab-minio:0.0.1 server /data --console-address :9090

sudo docker run -d --net=host --restart unless-stopped --name backend-container \
-p 8080:8080 \
-e DATABASE_URL=jdbc:postgresql://localhost:5432/edm \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=postgres \
-e SPRING_JPA_HIBERNATE_DDL_AUTO=update \
-e MINIO_ROOT_USER=minio-user \
-e MINIO_ROOT_PASSWORD=minio-password \
-e MINIO_ENDPOINT=http://localhost:9000 \
moongrail/caselab-backend:0.0.1

#### Другие команды
sudo systemctl start docker (Если вдруг не будет включён( поставил на авт включение))вщ
sudo systemctl status docker
```
