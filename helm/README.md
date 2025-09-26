# Dummy CronJob Helm Chart

This Helm chart deploys a dummy CronJob that executes every 5 minutes in a Kubernetes cluster.

## Description

The dummy CronJob runs a simple shell script that:
- Prints start and completion timestamps
- Performs a dummy sleep operation for 10 seconds
- Uses the lightweight `busybox` image

## Installation

### Install the chart
```bash
helm install my-dummy-cronjob ./dummy-cronjob-chart
```

### Install with custom values
```bash
helm install my-dummy-cronjob ./dummy-cronjob-chart -f custom-values.yaml
```

### Install in a specific namespace
```bash
helm install my-dummy-cronjob ./dummy-cronjob-chart -n my-namespace --create-namespace
```

## Configuration

The following table lists the configurable parameters and their default values:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `cronjob.schedule` | Cron schedule expression | `"*/5 * * * *"` (every 5 minutes) |
| `cronjob.image.repository` | Container image repository | `busybox` |
| `cronjob.image.tag` | Container image tag | `1.35` |
| `cronjob.successfulJobsHistoryLimit` | Number of successful jobs to keep | `3` |
| `cronjob.failedJobsHistoryLimit` | Number of failed jobs to keep | `1` |
| `resources.limits.cpu` | CPU limit | `100m` |
| `resources.limits.memory` | Memory limit | `128Mi` |
| `serviceAccount.create` | Create service account | `true` |

## Customization Examples

### Change the schedule to run every hour
```yaml
cronjob:
  schedule: "0 * * * *"
```

### Use a different container image
```yaml
cronjob:
  image:
    repository: alpine
    tag: "3.18"
  command:
    - /bin/sh
    - -c
    - echo "Running on Alpine Linux at $(date)"
```

### Adjust resource limits
```yaml
resources:
  limits:
    cpu: 200m
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 128Mi
```

## Monitoring

Use the following commands to monitor your CronJob:

```bash
# Check CronJob status
kubectl get cronjob

# View job history
kubectl get jobs

# Check logs from the latest job
kubectl logs -l job-name=$(kubectl get jobs -o jsonpath='{.items[-1:].metadata.name}')
```

## Uninstallation

```bash
helm uninstall my-dummy-cronjob
```

## Requirements

- Kubernetes 1.19+
- Helm 3.0+