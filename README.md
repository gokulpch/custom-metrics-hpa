# Custom Scaling of Applications - Prometheus Adapter, Prometheus Operator and HPA-Kubernetes

### Setting Up the Demo

Run ```quick_install.sh``` for getting all the objects required for the Demo (need helm on the workstation - https://helm.sh/docs/intro/install/)

### To demonstrate HPA

* Access the Demo App using the LoadBalancer IP and just hit the button on the page
* Describe the HPA object or the demo-app Deployment object to show the scaling action:

```
gokul.chandra@ ~/prometheus-custom-metrics  kubectl describe hpa prometheus-demo-app                                                                                                                                          
Name:                                           prometheus-demo-app
Namespace:                                      default
Labels:                                         <none>
Annotations:                                    <none>
CreationTimestamp:                              Fri, 20 Aug 2021 01:16:01 -0700
Reference:                                      Deployment/prometheus-demo-app
Metrics:                                        ( current / target )
  "demo_app_button_clicks_per_second" on pods:  0 / 1m
Min replicas:                                   1
Max replicas:                                   10
Deployment pods:                                1 current / 1 desired
Conditions:
  Type            Status  Reason            Message
  ----            ------  ------            -------
  AbleToScale     True    ReadyForNewScale  recommended size matches current size
  ScalingActive   True    ValidMetricFound  the HPA was able to successfully calculate a replica count from pods metric demo_app_button_clicks_per_second
  ScalingLimited  True    TooFewReplicas    the desired replica count is less than the minimum replica count
Events:
  Type    Reason             Age    From                       Message
  ----    ------             ----   ----                       -------
  Normal  SuccessfulRescale  9m34s  horizontal-pod-autoscaler  New size: 4; reason: pods metric demo_app_button_clicks_per_second above target
  Normal  SuccessfulRescale  9m19s  horizontal-pod-autoscaler  New size: 8; reason: pods metric demo_app_button_clicks_per_second above target
  Normal  SuccessfulRescale  9m3s   horizontal-pod-autoscaler  New size: 10; reason: pods metric demo_app_button_clicks_per_second above target
  Normal  SuccessfulRescale  2m36s  horizontal-pod-autoscaler  New size: 1; reason: All metrics below target
```

```
gokul.chandra@ADVISEEVOLVEITS~/prometheus-custom-metrics  kubectl describe deployment prometheus-demo-app                                                                                                                                         
Name:                   prometheus-demo-app
Namespace:              default
CreationTimestamp:      Fri, 20 Aug 2021 00:45:35 -0700
Labels:                 app=prometheus-demo-app
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=prometheus-demo-app
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=prometheus-demo-app
  Containers:
   prometheus-demo-app:
    Image:      flipstone42/k8s-prometheus-custom-scaling:latest
    Port:       8000/TCP
    Host Port:  0/TCP
    Limits:
      cpu:        100m
      memory:     128Mi
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   prometheus-demo-app-678b5994d6 (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  42m    deployment-controller  Scaled up replica set prometheus-demo-app-678b5994d6 to 1
  Normal  ScalingReplicaSet  8m50s  deployment-controller  Scaled up replica set prometheus-demo-app-678b5994d6 to 4
  Normal  ScalingReplicaSet  8m34s  deployment-controller  Scaled up replica set prometheus-demo-app-678b5994d6 to 8
  Normal  ScalingReplicaSet  8m19s  deployment-controller  Scaled up replica set prometheus-demo-app-678b5994d6 to 10
  Normal  ScalingReplicaSet  112s   deployment-controller  Scaled down replica set prometheus-demo-app-678b5994d6 to 1
```
