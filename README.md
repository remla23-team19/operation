# Operation 🏃

[![Latest Commit](https://img.shields.io/github/last-commit/remla23-team19/operation.svg)](https://github.com/remla23-team19/operation/commits/main) ![Docker Compose](https://img.shields.io/badge/repository-Docker%20Compose-blue)

All deployment files for the complete project.

## Instructions

Please follow the instructions below to run the project. You can either use Docker Compose or Kubernetes. The latter is recommended for a full experience of the project.

### Docker Compose

⚠️ Make sure to change the `docker-compose.yml` file to the correct image tags if you want to use a specific version of the [app](https://github.com/remla23-team19/app) or [model-service](https://github.com/remla23-team19/model-service).

1. Clone this repository:

```
git clone https://github.com/remla23-team19/operation.git
```

2. Build the Docker containers using the latest version:

```
docker-compose build --no-cache
```

> Note: always redo this step if you manually change version tags in the `docker-compose.yml` file. Otherwise, Docker will use the cached version.

3. Start the Docker containers:

```
docker-compose up
```

1. Open the web browser and go to:

```
http://localhost:9999
```

👉 Check sentiment of text (using our [app](https://github.com/remla23-team19/app)) based on the [model-service](https://github.com/remla23-team19/model-service).

Once you're finished, you can stop the process (`CTRL + C`) and the containers with:

```
docker-compose down
```

### Kubernetes

Using [Minikube](https://minikube.sigs.k8s.io/docs/start/), you can run the project locally as follows.

1. Start Minikube:

```
minikube start
```

2. Enable the [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) and metrics-server addons:

```
minikube addons enable ingress
minikube addons enable metrics-server
```

3. Clone this repository and go to the `k8s` folder:

```
git clone https://github.com/remla23-team19/operation.git
cd operation/k8s
```

4. Create the Kubernetes resources:

```
kubectl apply -f kubernetes.yml
```

5. Wait until all pods are running (this may take a few minutes), inspect the status with:

```
minikube dashboard
```

6. To access the app, you need to tunnel the Ingress controller to your local machine:

```
minikube tunnel
```

7. Now you can open the web browser and go to: [http://localhost](http://localhost)

> Note: in case you get a 502 error after submitting a query in the frontend quickly after tunneling and creating all resources, please wait a moment for model-service to be ready.

Once you're finished, you can delete the Kubernetes resources with:

```
kubectl delete -f deployment-test.yml
```

> Note: if you want to stop the dashboard and tunnel, you can do so with `CTRL + C`.
