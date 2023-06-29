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
kubectl delete -f kubernetes.yml
```

> Note: if you want to stop the dashboard and tunnel, you can do so with `CTRL + C`.

#### Monitoring 👀

With the help of [Helm](https://helm.sh/), you can install [Prometheus](https://prometheus.io/) and [Grafana](https://grafana.com/) to monitor the Kubernetes cluster.

1. Install Helm:

```
brew install helm
```

2. Add the Prometheus Repository and Install Prometheus Stack:

```
helm repo add prom-repo https://prometheus-community.github.io/helm-charts
helm repo update
helm install myprom prom-repo/kube-prometheus-stack
```

3. Access the dashboards via tunneling with Minikube:

```
minikube service myprom-kube-prometheus-sta-prometheus --url
minikube service myprom-grafana --url
```

> Note: this will provide a URL that you can open in your browser. To stop the tunnel, press `CTRL + C`. To delete the Prometheus stack, run `helm uninstall myprom`.

4. View metrics by going to the following page:

```
http://localhost/metrics
```

> Note: refresh the page to see the metrics change.

#### Istio
A second deployment using istio for traffic management can be installed and used as follows. Make sure to first follow [the installation instructions of istio](https://istio.io/latest/docs/setup/install/istioctl/)

1. Start Minikube (less memory also works):

```
minikube start --memory=16384 --cpus=4
```

2. Install Istio

```
istioctl install
```

3. Enable Istio injection
   
```
kubectl label ns default istio-injection=enabled
```

4. Enable the [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) and metrics-server addons:

```
minikube addons enable ingress
minikube addons enable metrics-server
```

5. Clone this repository and go to the `k8s` folder:

```
git clone https://github.com/remla23-team19/operation.git
cd operation/k8s
```

6. Add the Prometheus Repository and Install Prometheus Stack:

```
helm repo add prom-repo https://prometheus-community.github.io/helm-charts
helm repo update
helm install myprom prom-repo/kube-prometheus-stack
```

7. Apply the configuration to the cluster

```
kubectl apply -f kubernetes-istio.yml
```

8. Install Istio addons

```
kubectl apply -f istio-1.17.2/samples/addons/prometheus.yaml
kubectl apply -f istio-1.17.2/samples/addons/jaeger.yaml
kubectl apply -f istio-1.17.2/samples/addons/kiali.yaml
```
> Note: make sure you're inside the istio folder downloaded during [installation](https://istio.io/latest/docs/setup/install/istioctl/) when you run these.

9. Access the app

```
minikube tunnel
```

10. View dashboards

```
istioctl dashboard prometheus
istioctl dashboard kiali
```
