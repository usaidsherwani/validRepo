---
title: Destination Rules and Virtual Service

---
<!--Implementation of default destination and simple virtual service-->

In this demo we will see two different results. We will implement virtual service resources using Istio.

Istio helps in routing traffic based on constraints and rules defined by the user. We call "subsets" the routing destination.

Let's apply the default destination rule for the application, so if no other explicit rules are declared, these rules will be applied. 

For this demo, we will set the default destination rules to route all traffic equally to the microservice.

Take a look at the YAML file for default route configuration:

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: productpage
spec:
  host: productpage
  subsets:
  - name: v1
    labels:
      version: v1
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: reviews
spec:
  host: reviews
  subsets:
  - name: v1
    labels:
      version: v1
  - name: v2
    labels:
      version: v2
  - name: v3
    labels:
      version: v3
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: ratings
spec:
  host: ratings
  subsets:
  - name: v1
    labels:
      version: v1
  - name: v2
    labels:
      version: v2
  - name: v2-mysql
    labels:
      version: v2-mysql
  - name: v2-mysql-vm
    labels:
      version: v2-mysql-vm
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: details
spec:
  host: details
  subsets:
  - name: v1
    labels:
      version: v1
  - name: v2
    labels:
      version: v2
---
```

The resource kind `DestinationRule` uses the declarations mentioned in the code and applies those rules. For default behavior, we've taken a simple route to all components of the microservice.


Run the below command to apply the default destination route:

{{execute}}
```
kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml
```
{{/execute}}

Cross verify the same changes using the command below:

{{execute}}
```
kubectl get destinationrules -o yaml
```
{{/execute}}

Now, to apply dynamic or version-based routing, we have to implement a `VirtualService`. A Virtual Service is a important feature in traffic management. It adds flexibility in traffic distribution. With a Virtual Service, we can specify traffic behavior for multiple hosts. This feature is used when we want to send traffic to multiple versions of the same application or in the A/B testing approach. We can divide and distribute traffic in ratio and further analyze the application behavior. We mention the particular subset in the config file and distribute traffic.

Virtual service resource is important for the current scenario.

Refer to the YAML file below for the virtual service we're applying:

{{ copy filename='virtual-service.yaml' }}
```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: productpage
spec:
  hosts:
  - productpage
  http:
  - route:
    - destination:
        host: productpage
        subset: v1
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - route:
    - destination:
        host: ratings
        subset: v1
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: details
spec:
  hosts:
  - details
  http:
  - route:
    - destination:
        host: details
        subset: v1
---
```
{{ /copy }}

You can see the difference now in what we did. We created a default destination rule that routes traffic for the default scenario. Then we applied a Virtual Service route to send the traffic to only v1 of the application.

Run the below command to apply the YAML file for Virtual Service:

{{execute}}
```
kubectl apply -f virtual-service.yaml
```
{{/execute}}

Cross verify the same with the command below:

{{execute}}
```
kubectl get virtualservices -o yaml
```
{{/execute}}

We can test this traffic route by hitting the website. According to the rules, it will only divert the traffic to v1. Until here, we successfully did dynamic routing to a specific version of the application.

In the next step, we'll see how we can add identity-based routing rules.
