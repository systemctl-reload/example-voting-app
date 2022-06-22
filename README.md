# example-voting-app

This is based on the original [example-voting-app](https://github.com/dockersamples/example-voting-app) modified to work on my lab
```
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app
```
```
docker run -d --name=redis redis
```
```
docker run -d --name=db -e POSTGRES_HOST_AUTH_METHOD=trust postgres:9.4
```
```
docker run -d --name=worker --link db:db --link redis:redis worker-app
```
```
docker run -d --name=result -p 5001:80 --link db:db result-app
```


# docker-compose.yml
```
version: '3'
services:
  vote:
    image: voting-app
    container_name: vote
    ports:
      - '5000:80'
    depends_on:
      - redis
  redis:
    image: redis
    container_name: redis
  db:
    image: postgres:9.4
    container_name: db
    environment:
      - POSTGRES_HOST_AUTH_METHOD=trust
  worker:
    image: worker-app
    container_name: worker
    depends_on:
      - redis
      - db
  result:
    image: result-app
    container_name: result
    ports:
      - 5001:80
    depends_on:
      - db
```
