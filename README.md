# PoC-DORA-metrics
DORA metrics (for “DevOps Research and Assessment metrics”) measure deployment frequency, delivery speed, reliability, and recovery ability, providing a comprehensive view of DevOps capabilities. These metrics provide a standard set of DevOps metrics used to evaluate the process’s performance and maturity and help organizations improve software development and IT operations.

| DORA metrics                                                                               | Low                            | Medium                               | High                           | Elite                                         |
| ------------------------------------------------------------------------------------------ | ------------------------------ | ------------------------------------ | ------------------------------ | --------------------------------------------- |
| **Deployment frequency** <br> How often a deployment to production is successfully ?       | Fewer than once per six months | Once per month - Once per six months | Once per week - Once per month | On demand (possible multiple deploys per day) |
| **Deployment lead time** <br> The amount of time it takes a commit to get into production  | + 6 months                     | 1 month - 6 months                   | 1 day - 1 month                | Less than 1 hour                              |
| **Mean time to recovery** <br> How long it takes to recover from a failure in production   | + 1 week                       | 1 day - 1 week                       | Less than 1 day                | Less than 1 hour                              |
| **Change failure rate** <br> The percentage of deployments causing a failure in production | + 50%                          | + 30 %                               | + 15 %                         | 0–15 %                                        |

## Project purpose

This PoC aim to show the strengths and weaknesses of three dedicated tools (Keptn, DevLake and Pelorus) to retrieve and visualize DORA metrics. To simulate the deployment, I will use a simple Hello-World application (traefik:whoami).

We are looking for a tool to retrieve data from OCP (or k8s) but also other sources like Jenkins, Tekton, GitLab, ServiceNow, etc.

## Setup

For this project, I will use my own "Kubernetes Cluster". This cluster is made of 4 VM on a Proxmox server (3 k8s nodes and a master).

| Host           | CPU   | RAM   | Storage |
|----------------|-------|-------|---------|
| Proxmox server | 8 CPU | 16 Go | 67 Go   |
| k8s-master     | 4 CPU | 4 Go  | 16 Go   |
| k8s-node-1     | 2 CPU | 2 Go  | 16 Go   |
| k8s-node-2     | 2 CPU | 2 Go  | 16 Go   |
| k8s-node-3     | 2 CPU | 2 Go  | 16 Go   |

## k8s dashboard

```bash
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard

# To forward the port to access the dashboard
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```

I can now access the dashboard here : https://localhost:8443/#/login

## Tools
### Keptn

https://keptn.sh/stable/docs/guides/dora/

This tool can only retrieve data from Kubernetes

### DevLake

https://devlake.apache.org/docs/v1.0/Overview/Introduction

This tool can retrieve data from different source control tools (GitHub, GitLab, Bitbucket, etc.), automation tools (Jenkins, Azure DevOps) but also webhooks.

### Pelorus

https://pelorus.readthedocs.io/en/v2.0.12/GettingStarted/Overview/

Pelorus can retrieve data from OCP (not k8s), Git providers (GitHub, GitLab, Gitea), Azure DevOps and issue tracker (Jira, GitHub, ServiceNow, PagerDuty)