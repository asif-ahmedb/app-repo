# app-repo
Simple Application Repository

**Description**: The project contains two micro-services namely airports and countries which will run in a container environment. An nginx container has been used as reverse proxy to forward traffic to each micro-service.
```
├── README.md
├── airports-assembly-1.0.1.jar
├── airports-assembly-1.1.0.jar
├── countries-assembly-1.0.1.jar
├── dockerfile.airports
├── dockerfile.countries
├── dockerfile.nginx
└── nginx.conf
```
**Background**:  Provided are two different web API's for a fictional airport application, only GET requests are supported:

**1**. country-service: A service which returns basic information about countries
```      
          Endpoint: /countries to get a full list of countries
          Endpoint: /countries/<query> to search for country by name / ISO code.
```

**2**. airport-service:  A service which returns information about airports with country codes
```      
          Endpoint: /airports to get a full list of airports and their runways
          Endpoint: /airports/<query> to get a list of airports based on country code (e.g.: "NL")
```
**Version Update**
```      
          version 1.1.0: a service which returns information about airports with country codes
          Endpoint: /airports?full=[0|1] returns a summary or all details of all airports, depending on the value of full.
          Endpoint: /airports/<id> returns the full information of an airport based on its identifier. E.g.: /airports/EHAM returns all information for Schiphol.
          Endpoint: /search/<qry> returns a list of airports based on a country code search.
```
In addition to their application-specific endpoints all versions of both services expose the following two endpoints:
```           
          /health/live returns 200 when the HTTP server is up.
          /health/ready returns 200 when the service is done initializing and ready to serve requests, 503 when the service is still initializing.
```
**Prerequisites**
- Docker
- A container registry [if you would like to push the images manually]
  
**Steps to build locally**
- Clone the repo
- cd into the repo directory and run the docker build commands
```
  cd app-repo
  docker build -t <NAMESPACE>/<DOCKER_REPO>:<TAG> . -f <DOCKERFILE>
**For example**:
  1. docker build -t mynamespace/airports-assembly:1.0.1 . -f dockerfile.airports
  2. docker build -t mynamespace/countries-assembly:1.0.1 . -f dockerfile.countries
  3. docker build -t mynamespace/nginx:1.0.1 . -f dockerfile.nginx
```
`Docker public repositories has been used to push the images so that it can be easily pulled later without authentication. Also, we can use other container registries like Elastic Container Registry, Azure Container Registry or others. `

**On GitHub via GitHub Actions**

*Pre-requisites*: 

We need the credentials and access to push the images to docker registry; in this case, [hub.docker.com](https://hub.docker.com).
Please set the below as repository secrets:

- DOCKER_USERNAME
- DOCKER_PASSWORD

*GitHub Action in Action:*

When we push code to the "main" branch, GitHub action kicks in and builds the images, tags them and pushes them to the container registry.
`NOTE: For now hard-coded versions (For example; 1.0.1 or 1.1.0) are used for the tags but we can use commit-SHA or provide the version while running the job.`
