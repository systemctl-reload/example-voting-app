# example-voting-app

docker run -d --name=redis redis

docker run -d --name=db -e POSTGRES_HOST_AUTH_METHOD=trust postgres:9.4

docker run -d --name=vote -p 5000:80 --link redis:redis voting-app

docker run -d --name=result -p 5001:80 --link db:db result-app

docker run -d --name=worker --link db:db --link redis:redis worker-app
