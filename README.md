# Eddy AutoML Helm Charts

> Helm Charts for deploying all the components of Eddy AutoML.

## Deploying without Ingresses

1. Build dependecies
```
helm dependency build
```

2. Create a namespace
```
kubectl create ns eddy-automl
```

3. Install all the charts including a Kafka cluster
```
helm install -name demo --namespace eddy-automl  .
```

or 

3. Install only the Eddy AutoML charts wihout the Kafka cluster

```
helm install -name demo --namespace eddy-automl --set kafka.enabled=False .
```

## Deploying with Ingresses
