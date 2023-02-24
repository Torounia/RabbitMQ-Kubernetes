# RabbitMQ Deployed in Kubernetes

## Requirements

- [x] Docker and Kubernetes based
- [x] Package to Helm
- [ ] Sample app to add and retrieve data from the queue
- [ ] CICD pipeline with automatic testing
- [ ] Deploy to AWS EKS (infra provisioning using terraform)

### future goals

- [ ] proper secret management
- [ ] production ready (HA, clustering etc)
- [ ] load test

### Questions

1) why do we need mirroring in rabbitmq if we can't pull the data from all nodes?  

### Kubernetes

Flow: we need a service account for RabbitMQ to be able to use the kubernetes API and discover other peers (or nodes) in the cluster (this is required for clustering) and attach this service account via role binding to the namespace that hosts this deployment

A service account provides an identity for processes that run in a Pod, and maps to a Service Account object. It acts as the authentication method for the pod against the kubernetes API.

RabbitMQ peers authenticate each other using an Erlang Cookie. Erlang is a different type of programming language used for high traffic systems such telephony. This cookie can be set as an enviromental varialble in the namespace where all RabbitMQ pods will be running.

To set the cookie, generate the cookie, create a kubernetes secret object and specify the variable name RABBITMQ_ERLANG_COOKIE and paste the generated cookie value for before.

RabbitMQ clustering require that peer names (or pods) in the cluster have the same name and network identity everytime they start. For this reason, they require a statefulset provision.

### Helm

1) create the folder structure - helm create example-app
2) delete all files created as part of the bootstrap
3) add all kubernetes manifest files inside the templates folder
4) change directory and move to root folder that holds the helm folder
5) install the chart

### Sample app to interact with the Queue

Based in Go lang
