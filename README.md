# Flux Observability Platform

A GitOps observability stack for Kubernetes: metrics, logs and traces, declared in Git and reconciled by FluxCD.
Built as my thesis project during my internship at Cloudist AB (2026).

## Problem
Observability tooling that is set up by hand drifts over time and is hard to rebuild. I wanted a stack that can be recreated from a repository, with no manual `kubectl apply` after the initial bootstrap.

## Solution
- FluxCD bootstraps the cluster from this repository
- Traefik as ingress with TLS and path-based routing
- Prometheus and Alertmanager for metrics
- FluentBit ships logs to Loki
- Tempo receives traces over OTLP
- Grafana with dashboards provisioned from ConfigMaps
- Secrets are created manually per cluster and never committed to Git

## Stack
FluxCD · Kubernetes · Traefik · Prometheus · Alertmanager · Loki · FluentBit · Tempo · Grafana · Helm · Kustomize

## Result
The whole stack is described in Git and built in nine ordered parts (see the guide below). [Fill in: how many environments or clusters you ran it on, and anything you measured]

## What was hard
| Problem | What fixed it |
|---------|---------------|
| Tempo in `CrashLoopBackOff` | Pinned the chart to `1.23.3` instead of `1.24.x` |
| Loki `502 Bad Gateway` | Disabled the gateway and ran Loki in `SingleBinary` mode |
| Loki sidecar SSL error and crash | Pinned `sidecar.image.tag: 1.28.0` |
| FluentBit `domain not found` | Pointed it directly at `loki.loki.svc.cluster.local:3100` |
| Duplicate `HelmRelease` error | Moved the fluent repository to `infrastructure/configs/` |

The full step-by-step guide follows below.

---
