---
title: Prerequisite
---

Let's install Istio in the master node:

*This command will download the latest stable version. Currently, the latest version is 1.13.2.*

Run the below command to download the latest release:

{{ execute }}
```
curl -L https://istio.io/downloadIstio | sh -
```
{{ /execute }}

Switch to the Istio download directory:

{{ execute }}
```
cd istio-1.13.2
```
{{ /execute }}

Add `istioctl` client path to the path:

{{ execute }}
```
export PATH=$PWD/bin:$PATH
```
{{ /execute }}

Now let's install Istio and the services:

{{ execute }}
```
istioctl install --set profile=demo -y
```
{{ /execute }}

Now label the namespace so that the control plane can perform Istio injection in the application and detect it via this label:

{{ execute }}
```
kubectl label namespace default istio-injection=enabled
```
{{ /execute }}