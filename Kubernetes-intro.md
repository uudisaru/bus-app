# Kubernetes

Kubernetes is a open-source platform for running and coordinating containerized apps across a cluster of machines - orchestration engine.

Main benefits
- immutable infrastructure 
- declarative configuration
- scalability & high availability
- self-healing systems

Provides:
- service discovery and load balancing
- storage orchestration
- automated rollouts and rollbacks
- automatic bin packing
- self-healing
- secret and configuration management

## Kubernetes components and concepts

![Kubernetes components](docs/components-of-kubernetes.png)

### Kubernetes control plane

  Server or small group of servers that acts as a gateway and brain for the cluster; exposes API for managing the Kubernetes cluster.

  - etcd

    Globally available configuration store

  - kube-apiserver

    Main management point (with RESTful interface) of the cluter; acts as a bridge between components to maintain cluster health and disseminate information and commands.
    CLI tool kubectl is a default method to interact with Kubernetes cluster's API server.

  - kube-controller-manager

    Manages different controllers that regulate the state of the cluster, manage workload lifecycles etc. Monitors etcd to apply changed rules.

  - kube-scheduler

    Process that actually assigns workloads to nodes in cluster. Tracks available capacity on nodes.

  - cloud-controller-manager

    A layer that allows Kubernetes to interact with providers with different capabilities, features and API-s (e.g. private cluster, Amazon EKS, GKE etc).


### Kubernetes nodes

  Servers responsible for accepting and running workloads. Applications/services run in containers, so each node is equipped with container runtime like Docker.
  Servers create and destroy containers based on instructions from the control plane (kube-scheduler), adjusting networking rules to route and forward traffic appropriately.

  - Container runtime

    Docker, but also alternatives like rkt or runc.

  - kubelet

    Main contact point for each node in the cluster group. Relays information to and from control plane. Assumes responsibility for maintaining the state of work on node.

  - kube-proxy

    Forwards requests to the correct containers, can do primitive load balancing.

### Additional abstractions

  - Pods

    Basic unit that Kubernetes deals with. Containers are not assigned to hosts directly but to object called a pod. A pod represents one or more containers that should be controlled as a single application. Pods consist of containers that operate closely together, share a life cycle, and should always be scheduled on the same node.
    Usually pods consist of a main container that implements the general purpose of the workload and optionally some helper containers that facilitate closely related tasks (e.g. sidecars like used for authentication, service discovery etc).

  - Replication controllers and Replication Sets

    Often instead of working with individual pods you will instead manage groups of individual, replicated pods which are created from pod templates and can be horizontally scaled. Groups of pods are manage by controllers known as replication controllers and replication sets.

  - Deployments

    Pods, replication controllers and replication sets are rarely the units you will work with directly. 
    Deployments are one of the most common workloads to directly create and manage. Deployments are a high level object designed to ease the life cycle management of replicated pods. Deployments can be modified easily by changing the configuration and Kubernetes will adjust the replica sets, manage transitions between different application versions, and optionally maintain event history and undo capabilities automatically.

  - Stateful sets

    Specialized pod controllers that offer uniqueness guarantees; used mainly for workloads that require persistent data or stable networking (like databases which need access to the same volumes even if rescheduled to a new node).

### Other Kubernetes Components

  - Services

    In Kubernetes a service is a component that acts as a basic internal load balancer and ambassador for pods. A service groups together logical collections of pods that perform the same function to present them as a single entity.
    
    Services are by default available only by internally routable IP address, but they can be made available outside of cluster using either

    - NodePort

      Static port on each node’s external networking interface. Traffic to the external port will be routed automatically to the appropriate pods using an internal cluster IP service.

    - LoadBalancer

       External load balancer to route to the service using a cloud provider’s Kubernetes load balancer integration. The cloud controller manager will create the appropriate resource and configure it using the internal service service addresses.

  - Volumes and Persistent Volumes

    Volume is an abstraction that allows data to be shared by all containers within a pod and remain available until the pod is terminated.
    Persistent volumes are not tied to pod lifecycle; they allow administrators to configure storage resources for the cluster that users can request and claim for the pods they are running.

  - Labels and Annotations

    Key-value pairs attached to Kubernetes objects.
    Label is a semantic tag that is used to mark Kubernetes object as a part of a group. For example services use labels to understand the backend pods they should route requests to.

## Declarative configuration

See [demo](docs/gcloud-script.md).

## Additional resources

- kubeadm
- Kubernetes Secrets
- Sidecars
  - proxies or service meshes like Envoy, Istio, Nginx Ingress, Consul, linkerd
  - log shippers
  - monitoring agents etc
- Operators
  - deploying app on demand
  - taking/restoring backups
  - handling upgrades
  - etc
- Helm
- Knative
- Openshift
- Terraform
