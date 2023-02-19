# kubernetes-dev

### Set up the CloudWatch agent to collect cluster metrics
[source](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-setup-metrics.html)
- Step 1: Create a namespace for CloudWatch
  - `kubectl apply -f eks_dev/cloudwatch_agent/cloudwatch-namespace.yaml`
- Step 2: Create a service account in the cluster
    - `kubectl apply -f eks_dev/cloudwatch_agent/cwagent-serviceaccount.yaml`
- Step 3: Create a ConfigMap for the CloudWatch agent
  - `kubectl apply -f eks_dev/cloudwatch_agent/cwagent-configmap.yaml`
- Step 4: Deploy the CloudWatch agent as a DaemonSet
  - `kubectl apply -f eks_dev/cloudwatch_agent/cwagent-daemonset.yaml` <br>

when complete, the CloudWatch agent creates a log group named /aws/containerinsights/Cluster_Name/performance.

### Horizontal Pod Autoscaler
[source](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
- Step 1: create a cluster 
- Step 2: install kubectl metrics server
  - `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`
  - verify the installation of metrics server `kubectl get deployment metrics-server -n kube-system`
- Step 3; deploy the file 
  - `kubectl apply -f eks_dev/autoscaling/hpa-php-apache.yaml`
  - verify the deployment `kubectl get all`
- Step 4: increase the load 
```commandline
# Run this in a separate terminal
# so that the load generation continues and you can carry on with the rest of the steps
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```
We can monitor the pod usage by
```
kubectl get hpa php-apache --watch
```