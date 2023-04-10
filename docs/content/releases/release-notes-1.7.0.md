---
title: "Release Notes 1.7.0"
date: 02/03/2004
draft: false
toc: true
description: Release information for NGINX Service Mesh, a configurable, low‑latency infrastructure layer designed to handle a high volume of network‑based interprocess communication among application infrastructure services using application programming interfaces (APIs).  Lists of new features and known issues are provided.
weight: 500
categories: ["reference"]
---

## NGINX Service Mesh Version 1.7.0

02/03/2004

<!-- vale off -->

These release notes provide general information and describe known issues for NGINX Service Mesh version 1.7.0, in the following categories:

- [NGINX Service Mesh Version 1.7.0](#nginx-service-mesh-version-170)
  - [Updates](#updates)
  - [Vulnerabilites](#vulnerabilities)
  - [Resolved Issues](#resolved-issues)
  - [Known Issues](#known-issues)
  - {{< link "/about/tech-specs" "Supported Versions" >}}

<br/>
<br/>
<span id="170-updates"></a>

### **Updates**

NGINX Service Mesh 1.7.0 includes the following updates:
<br/><br/>


<span id="170-resolved"></a>

### **Vulnerabilities**


#### **Fixes**

This release includes vulnerability fixes for the following issues.
<br/>

- None

<br/>

<span id="170-cvefixes"></a>

#### **Third Party Updates**

This release includes third party updates for the following issues.
<br/><br/>

<br/>

<span id="170-thirdparty"></a>

### **Resolved Issues**

This release includes fixes for the following issues.
<br/><br/>


<br/>

<span id="170-issues"></a>

### **Known Issues**

The following issues are known to be present in this release. Look for updates to these issues in future NGINX Service Mesh release notes.
<br/>


<br/>**Lingering invalid RateLimits can cause restart inconsistencies with the NGINX Service Mesh control plane. (658)**:
  <br/>

The NGINX Service Mesh control plane has a validating webhook that will reject the majority of RateLimits that conflict with an existing RateLimit. However, there are some cases where the validating webhook is unable to determine a conflict. In these cases, the NGINX Service Mesh control plane process will catch the conflict and prevent configuration of the offending RateLimit, but the RateLimit will still be stored in Kubernetes. These RateLimit resources are invalid and can be found by looking for a `Warning` event on the RateLimit object. If invalid RateLimits exist and the NGINX Service Mesh control plane restarts, the invalid RateLimits may be configured over the previous valid RateLimits. 
  <br/>
  <br/>
  Workaround:
  <br/>

When you create a RateLimit resource in Kubernetes, run `kubectl describe ratelimit <ratelimit-name>` and check for any `Warning` events. If a `Warning` event exists, either fix the conflict described in the `Warning` event message, or delete the RateLimit by running: `kubectl delete ratelimit <ratelimit-name>`. 
  

<br/>**Duplicate targetPorts in a Service are disregarded (532)**:
  <br/>

NGINX Service Mesh supports a variety of Service `.spec.ports\[]` configurations and honors each port list item with one exception.

If the Service lists multiple port configurations that duplicate `.spec.ports\[].targetPort`, the duplicates are disregarded. Only one port configuration is honored for traffic forwarding, authentication, and encryption.

Example invalid configuration:


```yaml
apiVersion: v1
kind: Service
spec:
ports:
- port: 8080
     protocol: TCP
     targetPort: 55555
- port: 9090
     protocol: TCP
     targetPort: 55555
```
  <br/>
  <br/>
  Workaround:
  <br/>

No workaround exists outside of reconfiguring the Service and application. The Service must use unique `.spec.ports[].targetPort` values (open up multiple ports on the application workload) or route all traffic to the application workload through the same Service port.
  

<br/>**NGINX Service Mesh DNS Suffix support (519)**:
  <br/>

NGINX Service Mesh only supports the `cluster.local` DNS suffix. Services such as Grafana and Prometheus will not work in clusters with a custom DNS suffix.
  <br/>
  <br/>
  Workaround:
  <br/>

Ensure your cluster is setup with the default `cluster.local` DNS suffix.
  

