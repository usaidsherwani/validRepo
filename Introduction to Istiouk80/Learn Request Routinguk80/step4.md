---
title: Dynamic Routing

---
<!--Implementation of dynamic routing-->

We created a virtual service that diverts the traffic to a particular version. Now let's configure the dynamic routing feature. We will take one of the use cases of user identity-based routing. This means two users will get different results even though they hit the same application because we've set the dynamic routing rule.

We will create a similar virtual service just as we did in the previous step. But here, we will add the `end-user` keyword and put conditions on the request header. So, if the request matches the mentioned end-user then it'll show some specific version, otherwise, it'll use the regular destination.

Refer to the YAML file below to apply dynamic route virtual service resource:

{{ copy filename='virtual-service-reviews.yaml' }}
```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
    - reviews
  http:
  - match:
    - headers:
        end-user:
          exact: brainarator
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
```
{{ /copy }}

Run the command below to apply the virtual service changes:

{{ execute }}
```
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews.yaml
```
{{ /execute }}

Now cross verify the same changes with the command below:

{{ execute }}
```
kubectl get virtualservice reviews -o yaml
```
{{ /execute }}

Here, for the reviews' microservice, if the header request matches the end-user named `brainarator` then it'll send the traffic to v2, else to version1. We set conditions for host and particular version which is also called subset.

This is how we can implement user-identity-based traffic distribution. This function helps if we have a separate group of users and want to serve different content to them. In that case, we can add the above identity routing approach and test out the use case.