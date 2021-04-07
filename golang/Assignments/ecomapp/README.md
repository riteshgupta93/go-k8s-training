## start postgres to serve as backend db

`docker run -d --name some-postgres -p 5432:5432 -e POSTGRES_PASSWORD=postgres -e PGDATA=/var/lib/postgresql/data/pgdata -v /custom/mount:/var/lib/postgresql/data postgres`


## Env variables required

export DBHOST=localhost

export DBPORT=5432

export DBUSER=postgres

export DBPASS=postgres

export DBNAME=postgres

export ORDERSERVICEHOST=localhost

export ORDERSERVICEPORT=8001

## start order service
`cd src/orders/ && go run api.go`

service will start on 8001 port

## start user service
`cd src/users/ && go run api.go`

service will start on 8000 port

## use following curl commands to populate data.
### create user
`curl localhost:8000/api/users -d '{"username":"ritesh", "contact":"82829898"}'`

note down the user_id which will be in response

### place order
`curl localhost:8000/api/users/placeorder -d '{"item":"milkshake", "user":"<< user_id >>"}'`

## get orders
`curl localhost:8000/api/users/<<user_id>>/orders`
 

### Start ecom app using docker-compose
`docker-compose -f docker-compose.yaml up -d`

user service will start on 8000 port

### Deploy app on kubernetes using helm
`cd helm/ecom-app && helm install ecom-app ./`

### Deploy app using kube operator locally on minkube, install operator-sdk

Used: operator-sdk version: "v1.5.0", commit: "98f30d59ade2d911a7a8c76f0169a7de0dec37a0", kubernetes version: "1.19.4", go version: "go1.15.5", GOOS: "linux", GOARCH: "amd64"

` cd kube-operator `

` make install run `

` kubectl apply -f config/samples/backend_v1alpha1_ecomapp.yaml `

### Some other useful command for using operator-sdk
Init new project using helm

`operator-sdk init --plugins helm --domain ecom.app --group backend --version v1alpha1 --kind EcomApp`

Build docker image of the operator

`make docker-build docker-push IMG=riteshgupta/ecom-app:kube-operator-0.1`

Deploy operator on kube cluster. This will use existing ~/.kube/config file

`make deploy IMG=riteshgupta/ecom-app:kube-operator-0.1`






