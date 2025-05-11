# grafana--dashboard
grafana -dashboard


✅ Step-by-Step: Create xDS Error Alert (via Console)
1. Create a Logs-based Metric
This captures xDS-related errors from Envoy (istio-proxy).

Go to Logging → Logs Explorer:
https://console.cloud.google.com/logs/query

Use this query (adjust for your setup):

sql
Copy
Edit
resource.type="k8s_container"
resource.labels.container_name="istio-proxy"
severity="ERROR"
textPayload:("xds" AND "error")
Click "Save as Metric" (upper right).

Name: xds_error_logs
Type: Counter
Unit: 1
Description: "Tracks xDS errors from Istio sidecars"

2. Create an Alerting Policy
Go to Monitoring → Alerting → Create Policy:
https://console.cloud.google.com/monitoring/alerting

Add Condition → Select “Log-based metric”

Metric: logging/user/xds_error_logs

Resource Type: Kubernetes Container

Configuration:

Rolling window: 1 minute

Condition: count > 1

Alignment Period: 1 minute

Aligner: rate

Reducer: sum

Notification Channels: Add email, Slack, xMatters, etc.

Name the policy: xDS Error Alert

Documentation (optional): Add runbook or instructions for mitigation.

Click Save.
