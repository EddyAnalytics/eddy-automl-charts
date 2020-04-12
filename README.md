# Eddy AutoML Helm Charts

> Helm Charts for deploying all the components of Eddy AutoML.

## Deploying without Ingresses

Build dependent charts
```
helm dependency build
```

Create a namespace
```
kubectl create ns eddy-automl
```

Install all the charts including a Kafka cluster
```
helm install -name demo --namespace eddy-automl  ./
```

or 

Install only the Eddy AutoML charts wihout the Kafka cluster

```
helm install -name demo --namespace eddy-automl --set kafka.enabled=False ./
```

## Deploying with NodePorts

Deploy all the components:

```
helm install -name demo --namespace eddy-automl -f values.minikube.yaml ./
```

If you are using minikube, it exposes all services of type NodePort to the host.

Run the following command to get the ports and update the `eddy-automl-ui-config` ConfigMap accordingly.

```
minikube service list
```

Example:
```
|-------------|--------------------------------|--------------|----------------------------|
|  NAMESPACE  |              NAME              | TARGET PORT  |            URL             |
|-------------|--------------------------------|--------------|----------------------------|
| default     | kubernetes                     | No node port |
| eddy-automl | demo-eddy-automl-backend       | http/8000    | http://192.168.64.10:30887 |
| eddy-automl | demo-eddy-automl-ui            | http/80      | http://192.168.64.10:31951 |
| eddy-automl | demo-eddy-kafka-graphql-bridge | http/8090    | http://192.168.64.10:32306 |
```

```
kubectl edit configmap demo-eddy-automl-ui-config

```

```json
    {
      "debug": false,
      "dataGraphQLClient": {
        "httpEndpoint": "http://192.168.64.10:30887/api/graphql",
        "wsEndpoint": "http://192.168.64.10:32306/subscriptions"
      }
    }
```

And rollout restart the deployment:

```
kubectl rollout restart deployment demo-eddy-automl-ui 

```

And open the UI at: http://192.168.64.10:31951

## Deploying with Ingresses (minikube)

Enable the `ingress` and `ingress-dns` addons by running:

```
minikube addons enable ingress
```

Run `minikube ip` and add edit `values.minikube-ingress.yaml` by replacing all the occurence of the IP.

Deploy the components:

```
helm install -name demo --namespace eddy-automl -f values.minikube.yaml -f values.minikube-ingress.yaml ./
```

Open the UI at: `http://automl-ui.<minikube-ip>.nip.io`, for example: http://automl-ui.192.168.64.10.nip.io

## Deploying with Ingresses (ngnix-ingress)

Requires proper DNS Zone configuration, `cert-manager` and `ngnix-ingress`. 

```
helm install -name demo --namespace eddy-automl -f values.production.yaml ./
```

Runing instances of these charts can be found at: 

https://automl.k.eddy-analytics.org
https://eddy-automl-backend.k.eddy-analytics.org
https://eddy-automl-subscriptions.k.eddy-analytics.org

## Uninstalling

```
helm uninstall demo
```