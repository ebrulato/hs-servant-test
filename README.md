# hs-servant-test

This repository contains a simple server based on _**servant**_, and deploy it on **Google Cloud Run**, with **Knative**.

## Local Test
===

### Build
To build the server.
```sh
stack build
```
This command may generate a local *.cabal* file.

### Run the Server localy
Launch the server on your local machine
```sh
stack run
```

### TODO
This code may have some improvement:
* Usage of the environment variable **PORT** to avoid PORT hard code.

## Google Cloud Run
===
### Build the knative image
You don't have to build the docker image localy.
First of all, you should check that the current project is the right one with the command:
```sh
gcloud config get-value project
```
set a big timeout value (31mn)

gcloud builds submit --tag gcr.io/[PROJECT-ID]/hs-servant-test --timeout=1h

## deployment

gcloud beta run deploy --image gcr.io/[PROJECT-ID]/hs-servant-test


## To build the docker image

Delete the cabal file to use correctly the stack configuration
check the memory usage

docker build -t ebrulato/hs-servant-test .


docker login

if error :
mv /usr/local/bin/docker-credential-osxkeychain /usr/local/bin/docker-credential-osxkeychain-old
(see https://github.com/docker/for-mac/issues/2295)

docker push ebrulato/hs-servant-test

local deployment (not tested)

kubectl apply --filename service.yaml
