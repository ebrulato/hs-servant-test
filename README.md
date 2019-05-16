# hs-servant-test

This repository contains a simple server based on _**servant**_, and deploy it on **Google Cloud Run**, with **Knative**.

## Local Test

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
Then you can execute a simple *curl* command to check that everything is alright.
```sh
curl http://localhost:8080/users
[{"email":"isaac@newton.co.uk","registrationDate":"1683-03-01","age":372,"name":"Isaac Newton"},{"email":"ae@mc2.org","registrationDate":"1905-12-01","age":136,"name":"Albert Einstein"}]
```
### TODO
This code may have some improvement:
* Usage of the environment variable **PORT** to avoid PORT hard code.

## Google Cloud Run

Note that you have to activate this service on on your project.
### Build the knative image
You don't have to build the docker image localy.
First of all, you should check that the current project is the right one with the command:
```sh
gcloud config get-value project
```
Then you have to launch the build remotely. Note that the operation with haskell is verrrrrry long (~30mn). So you have to set a big timeout value, to avoid the interruption of the process. In the next command, you have to update the *PROJECT_ID*.
```sh
gcloud builds submit --tag gcr.io/[PROJECT-ID]/hs-servant-test --timeout=40m
```
## Deployment
```sh
gcloud beta run deploy --image gcr.io/[PROJECT-ID]/hs-servant-test
```
You can also use the web console interface to do this.


## To build the docker image
This build will be performed locally.
Be sure to have enougth memory (and close other app if necessary).
Delete the cabal file to use correctly the stack configuration
```sh
docker build -t [DOCKER_ID]/hs-servant-test .
```
Then you have to login your docker account
```sh
docker login
```
*if this raise an unexpected error you may have to perform this strange operation...
mv /usr/local/bin/docker-credential-osxkeychain /usr/local/bin/docker-credential-osxkeychain-old
(see https://github.com/docker/for-mac/issues/2295)*

```sh
docker push ebrulato/hs-servant-test
```

Then you may perform local deployment (not tested).
