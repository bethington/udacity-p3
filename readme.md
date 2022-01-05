# Udacity Project 3 - Refactor Udagram App into Microservices and Deploy

Udagram is a simple cloud application developed along side the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

Refactoring [udagram restapi monolitic](https://github.com/udacity/cloud-developer/tree/master/course-02/exercises/udacity-c2-restapi)and [udagram frontend monolitic](https://github.com/udacity/cloud-developer/tree/master/course-02/exercises/udacity-c2-frontend) application into separate microservices as follows:

The project is split into four parts:
1. [Simple Frontend](https://github.com/bethington/prodject-3/tree/master/course-02/exercises/udacity-c2-frontend)
A basic Ionic client web application which consumes the RestAPI Backend. 
2. [RestAPI Feed Backend](https://github.com/bethington/prodject-3/tree/master/course-02/exercises/udacity-c2-restapi), a Node-Express server which can be deployed to a cloud service.
3. [RestAPI User Backend](https://github.com/bethington/prodject-3/tree/master/course-02/exercises/udacity-c2-restapi), a Node-Express server which can be deployed to a cloud service.
4. [The Image Filtering Microservice](https://github.com/bethington/prodject-3/tree/master/course-02/project/image-filter-starter-code), the final project for the course. It is a Node-Express application which runs a simple script to process images.

- Divide the application into smaller services and extend it with additional service and automatic continuous integration and continuous delivery.
- Containerize the application, create the Kubernetes resource and deploy it to a Kubernetes cluster.
- Extend the application with deployments and be able to do rolling-updates and rollbacks.
***

## PROJECT SPECIFICATION

## Usage
### Udagram Simple Frontend Setup

#### Installing Node and NPM
This project depends on Nodejs and Node Package Manager (NPM). Before continuing, you must download and install Node (NPM is included) from [https://nodejs.com/en/download](https://nodejs.org/en/download/).

#### Installing Ionic Cli
The Ionic Command Line Interface is required to serve and build the frontend. Instructions for installing the CLI can be found in the [Ionic Framework Docs](https://ionicframework.com/docs/installation/cli).

#### Installing project dependencies
This project uses NPM to manage software dependencies. NPM Relies on the package.json file located in the root of this repository. After cloning, open your terminal and run:
```bash
npm install
```

#### Configure The Backend Endpoint
Ionic uses enviornment files located in `./src/enviornments/enviornment.*.ts` to load configuration variables at runtime. By default `environment.ts` is used for development and `enviornment.prod.ts` is used for produciton. The `apiHost` variable should be set to your server url either locally or in the cloud.

***

### Installation Windows 10
- Prerequisite: It is best to have Ubuntu 18.04, Docker 18.09, KubeOne v0.9.0, Terraform 0.12.8 installed before starting the project.
- Once the repository has been downloaded, you will need to first install all dependencies by running `npm i`. You will need to repeat this for all `Frontend`, `Restapi-Feed` and `Restapi-user` folders.
- Also, ensure all environment variables are set up in your `~/.profile`.
#### Setup Docker Environment
You'll need to install docker https://docs.docker.com/install/. Open a new terminal within the project directory and run:

1. Switch the folder: `cd deployment/docker`
2. Build the images: `docker-compose -f docker-compose-build.yaml build --parallel`
3. Push the images: `docker-compose -f docker-compose-build.yaml push`
4. Run the container: `docker-compose up`

#### Setup k8s Environment

1. Create the cluster: `eksctl create cluster --name udagram`
2. Create travis-user: `eksctl create iamidentitymapping --name  udagram --role arn:aws:iam::?:role/travis_eks --group system:masters --username travis_eks`
3. Run the ci/cd-pipeline
- For Docker:
    - First, install Docker in your system.
    - Build images using `docker-compose -f docker-compose-build.yaml build --parallel`
    - Push the docker images to DockerHub using `docker-compose -f docker-compose-build.yaml push`.
    - Run application locally using `docker-compose -f docker-compose.yaml up`. **Note** you have to export enviroment varibales needed for the application first. Fill *secrets.example.sh* and Run `source secrets.example.sh`
    - Go to Localhost:8100 to test.
    - Docker deployment complete.
- For Kubernetes:
    - Install Kubernetes using the Kubeone instructions given here:  `https://github.com/kubermatic/kubeone/blob/master/docs/quickstart-aws.md`
    - Follow the setup videos closely, and export the KubeConfig.
    - Check for the proper setup using `kubectl get pods`.
    - Set up the secret keys and configmaps for the project. These are the .yaml files in the k8s folder.
    - Make sure the .yaml files are also pointing to the right Docker images which you set up previously.
    - Use `kubectl apply -f [file-name]` on the secret key, config map files.
    - Use `kubectl apply -f [file-name]' again for the remaining .yaml files. If all is set up correctly, you will see all pods running. Test using `kubectl get rs`, `kubectl get deployment`, and `kubectl get pods`.
    - Forward the deployment port to Localhost:8080 using `kubectl port-forward [name]/reverseproxy-[id] 8080:8080`.
    - Go to Localhost:8080 to test. 
    - Kubernetes deployment completed.
- For Travis:
    - Ensure your Github account is linked to Travis. This is done via signing up for a Travis account, then from within linking the two together.
    - Push your folders to Github repository. No need for node_modules nor secrets to be included in the repository. 
    - Ensure the .travis.yml file is in the root folder of the Github repo. 
    - Go to Travis, then search for the repository, and then set up new build.
    - Travis deployment completed.
***

### Installing useful tools
#### [Postbird](https://github.com/paxa/postbird)
Postbird is a useful client GUI (graphical user interface) to interact with our provisioned Postgres database. We can establish a remote connection and complete actions like viewing data and changing schema (tables, columns, ect).

#### [Postman](https://www.getpostman.com/downloads/)
Postman is a useful tool to issue and save requests. Postman can create GET, PUT, POST, etc. requests complete with bodies. It can also be used to test endpoints automatically. We've included a collection (`./udacity-c2-restapi.postman_collection.json `) which contains example requsts.

### Running Locally
#### Running the Backend Development Server Locally
To run the server locally in developer mode, open terminal and run:
```bash
npm run dev
```

#### Running the Frontend Development Server Locally
Ionic CLI provides an easy to use development server to run and autoreload the frontend. This allows you to make quick changes and see them in real time in your browser. To run the development server, open terminal and run:

```bash
ionic serve
```

### Building
#### Building the Static Frontend Files
Ionic CLI can build the frontend into static HTML/CSS/JavaScript files. These files can be uploaded to a host to be consumed by users on the web. Build artifacts are located in `./www`. To build from source, open terminal and run:
```bash
ionic build
```
***

## Dependencies

There are dependencies on having an S3 bucket provisioned and an RDS postgres instance provisioned if you want to run this locally on your machine

### Images for evidence
- Attached are the following images which demonstrate the functionality of the project:
    - Running application;
    - DockerHub images;
    - Kubernetes running pods; and
    - Travis deployment completed.

## @TODO
Tasks:
- Frontend Setup:
    - [ ] Clone, set up protected branches (dev, staging, master)
    - [ ] NPM, Ionic CLI
    - [ ] run tests (npm test), identify broken function, fix the function
    - [ ] write tests for form validation and re-run tests