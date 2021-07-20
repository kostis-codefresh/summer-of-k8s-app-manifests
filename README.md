# Summer of K8s Example application for Argo Rollouts

This repository contains the Kubernetes manifest
for the [summer of K8s](https://www.getambassador.io/summer-of-k8s) Argo rollouts example.

The actual source code is at [https://github.com/kostis-codefresh/summer-of-k8s-app](https://github.com/kostis-codefresh/summer-of-k8s-app)

## Prerequisite

1. Get access to a Kubernetes cluster
2. Install Argo Rollouts following the [instructions](https://argoproj.github.io/argo-rollouts/installation/). Be sure to install the CLI as well
3. Install Ambassador Edge stack following the [instructions](https://www.getambassador.io/docs/edge-stack/latest/tutorials/getting-started/)

## Making the first deployment

```
git clone https://github.com/kostis-codefresh/summer-of-k8s-app-manifests
cd summer-of-k8s-app-manifests
kubectl create namespace demo
kubectl apply -f . -n demo
```

Wait for some time for the application to come up
```
kubectl get pods -n demo
```

Get the ambassdor URL 
```
kubectl get svc ambassador -n ambassador
```

Note down the "External IP" URL (for example 32.98.176.18)


Visit the application at https://32.98.176.18/demo/

## Make a rollout canary release

Open a second terminal and watch the canary

```
kubectl argo rollouts get rollout summer-k8s-rollout -n demo -w
```

Edit file rollout.yaml
and change line `kostiscodefresh/summer-of-k8s-app:v1` to `kostiscodefresh/summer-of-k8s-app:v2`

Deploy the second version

```
kubectl apply -f rollout.yaml -n demo
```

See the canary status at the second terminal. It should be at 30%. 
Take a screenshot to submit later in the evaluation phase.

If you also visit the application again you should see some boxes
with the new version 

![Request dashboard](https://raw.githubusercontent.com/kostis-codefresh/summer-of-k8s-app/main/dashboard.png)

Take a screenshot to submit later in the evaluation phase.

Promote the canary to 60% with

```
kubectl argo rollouts promote summer-k8s-rollout -n demo
```

Check the dashboard and terminal again

Promote the canary two more times to reach 100%

```
kubectl argo rollouts promote summer-k8s-rollout -n demo
```

Now the old version is no longer present

Submit your solution at [https://www.getambassador.io/summer-of-k8s/ship/week3/](https://www.getambassador.io/summer-of-k8s/ship/week3/)






