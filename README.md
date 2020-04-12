# Eddy AutoML Helm Charts

Helm Charts for deploying all the components of Eddy AutoML.

1. Build dependecies
```
helm dependency build
```

2. Install all the charts including a Kafka cluster
```
helm install -name demo --namespace eddy-automl  .
```

or 

2. Install only the Eddy AutoML charts wihout the Kafka cluster

```
helm install -name demo --namespace eddy-automl --set kafka.enabled=False .
```