---
title: Introduction to Traffic Management with Istio
---
<!--Introduction-->

Istio comes with many functionalities like traffic management, security configuration and enhancement in monitoring. In this learning path, we'll explore the traffic management feature with hands-on examples.

One of the use cases of Istio in traffic management is dynamic request routing. We will take the same demo example of [bookinfo microservice](https://istio.io/latest/docs/examples/bookinfo/) throughout the learning path. This demo application consists of multiple versions and those are running concurrently to observe the difference easily.

Let's start by understanding the use case of dynamic request routing. 

We are going to expose microservices publicly. End-users send and receive web requests to and from these microservices. 

By default, Kubernetes proxy will route traffic equally to the underlying Pods. It uses the round-robin algorithm for traffic distribution.


This approach is not much feasible when we want to deal with applications having multiple versions. There may be multiple reasons for deploying multi-version applications but using the default routing algorithm is not a good approach.

So, we can use this function of Istio where we can route the traffic request to a particular resource eventually a version of the application. We can customize the HTTP header and create rules based on conditions like routing based on specific browser requests or user identity.

Let's jump to the hands-on part and learn by doing!