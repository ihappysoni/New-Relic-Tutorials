New Relic Metrics Adapter

We can use metrics from our New Relic account to auto scale applications and services in our Kubernetes cluster by deploying the New Relic Metrics Adapter. This adapter fetches the metric values from New Relic and makes them available for the Horizontal Pod Auto Scalers.

Horizontal Pod Auto Scalers : Horizontal scaling means that the response to increased load is to deploy more Pods. This is different from vertical scaling, which for Kubernetes would mean assigning more resources (for example: memory or CPU) to the Pods that are already running for the workload.
If the load decreases, and the number of Pods is above the configured minimum, the Horizontal Pod Auto Scaler instructs the workload resource (the Deployment, Stateful Set, or other similar resource) to scale back down.
Steps to Configure Metrics Adapter:

Prerequisite:
1.	New Relic License Key
2.	Kubernetes Integration (Click 👈 there to see how to integrate the Kubernetes )

There is an HPA consuming the external metric as follows:
Note: Browse the directory where YAML file exists for the Docker Container which is previously used for integrating Kubernetes.




1.	Create a YAML file with any name (ie values.yaml ).
Put the below code :

kind: HorizontalPodAutoscaler
apiVersion: autoscaling/v2
metadata:
  name: nginx-scaler
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: External
      external:
        metric:
          name: nginx_average_requests
          selector:
            matchLabels:
              k8s.namespaceName: nginx
        target:
          type: Value
          value: 10000


2.	Make sure your Kubernetes integration is done properly and New Relic Account is getting the logs and metrics data.

3.	Give the following command to configure the HPA which will start the Kubernetes Service with name (horizontalpodautoscaler.autoscaling/nginx-scaler)

kubectl apply -f values.yaml

Note: The desired output will be :
horizontalpodautoscaler.autoscaling/nginx-scaler created

For More Info:
Visit: https://cloudeq-my.sharepoint.com/:w:/p/happy_soni/Edjb2uKgDl1EqJ8e8b-NpSwBwziwhIQEp_OiQ9ZvdP2wLA?e=nfaBJQ
