# kubernetes-dev

## Set up the CloudWatch agent to collect cluster metrics
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