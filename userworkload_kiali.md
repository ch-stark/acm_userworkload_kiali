# Mastering User Workload Monitoring with MCOA: A Multi-Cluster Kiali Example

The introduction of the MultiCluster Observability Addon (MCOA) in Red Hat Advanced Cluster Management (RHACM) represents a massive architectural leap for managing telemetry across hybrid cloud fleets. By replacing legacy custom components with established upstream standards like the Prometheus Agent, MCOA delivers a highly scalable, GitOps-friendly monitoring engine.

While MCOA automatically collects standard platform metrics for cluster health, monitoring your actual applications requires configuring User Workload Monitoring (UWM). To demonstrate the true power of this architecture, let's explore how to configure UWM with MCOA using a real-world, high-value use case: observing a multi-cluster Istio Service Mesh with Kiali.

---

## The Challenge: Multi-Cluster Service Mesh Blind Spots

Scaling a service mesh from one cluster to many multiplies your observability blind spots. Tracking a transaction as it crosses cluster boundaries turns debugging into a time-consuming detective hunt.

Kiali solves this by providing a single management console and network graph over your entire multi-cluster service mesh. However, for Kiali to present an end-to-end visualization, it needs a unified metrics store—it cannot query dozens of isolated clusters simultaneously.

By combining OpenShift's UWM, RHACM's MCOA, and Kiali, we can aggregate mesh telemetry centrally on the hub cluster and visualize fleet-wide traffic seamlessly.

---

## Step 1: Enable User Workload Monitoring in MCOA

First, we must instruct MCOA to deploy the necessary user workload agents across the fleet. You do this by editing the `MultiClusterObservability` custom resource (CR) on your hub cluster to enable user workload collection.

```yaml
apiVersion: observability.open-cluster-management.io/v1beta2 
kind: MultiClusterObservability 
metadata: 
  name: observability 
spec: 
  capabilities: 
    userWorkloads: 
      metrics: 
        collection: 
          enabled: true # Enables the UWM Prometheus Agents
