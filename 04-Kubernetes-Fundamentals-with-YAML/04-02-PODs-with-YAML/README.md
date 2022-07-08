# PODs with YAML

## Step-01: Kubernetes YAML Top level Objects

- Discuss about the k8s YAML top level objects
- **01-kube-base-definition.yml**

```yml
apiVersion:
kind:
metadata:
    name: :::no-loc text="":::
    labels: :::no-loc text="":::
    namespace: :::no-loc text="":::
spec:
```

[Kubernetes YAML Top Level Objects](https://kubernetes.io/docs/tutorials/kubernetes-basics/kubernetes-yaml/kubernetes-yaml-top-level-objects/)

- Kind: Types of Kubernetes Objects - Pod, ReplicaSet, Deployment, Service and many more
- apiVersion: version of k8s objects - Pick from K8s documentation below <'format: group/version'> (E.g.. Core/V1 for Pods, App/V1 for Deployments, etc.)
  - See [Pod API Objects Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#pod-v1-core)
- metadata: define name and labels for k8s objects
  - name: string - mandatory for all k8s objects
  - namespace: string - mandatory for all k8s objects
  - labels: dictionary - mandatory for Pods
    - app: :::no-loc text="":::
    - version: :::no-loc text="":::
    - environment: :::no-loc text="":::
    - owner: :::no-loc text="":::
    - team: :::no-loc text="":::
    - component: :::no-loc text="":::
    - name: :::no-loc text="":::
    - type: :::no-loc text="":::
    - role: :::no-loc text="":::
    - status: :::no-loc text="":::
    - state: :::no-loc text="":::
    - region: :::no-loc text="":::
    - location: :::no-loc text="":::
- spec: specification or core concrete definition for k8s objects included in metadata:
  - For a Pod definition, the spec is a container definition - see [Container Definition](https://kubernetes.io/docs/tutorials/in-depth-kubernetes-application-development-user-guide/#container-definition)
    - containers: list of containers
      - image: image name
      - name: container name
      - command: list of command line arguments
      - args: list of command line arguments
      - env: list of environment variables
        - name: environment variable name
        - value: environment variable value
      - ports: list of ports
      - volumeMounts: list of volume mounts
        - name: volume mount name
        - mountPath: path to mount volume to
        - readOnly: boolean
        - subPath: sub path to mount volume to
        - mountPropagation: propagation mode for volume mount
        - subPathExpr: sub path expression to mount volume to
      - resources: resource requirements
        - limits: dictionary of limits
          - cpu: :::no-loc text="":::
          - memory: :::no-loc text="":::
        - requests: dictionary of requests
          - cpu: :::no-loc text="":::
          - memory: :::no-loc text="":::
    - volumes: list of volumes
    - imagePullSecrets: list of imagePullSecrets
    - restartPolicy: restart policy for the pod
    - serviceAccountName: service account name for the pod
    - securityContext: security context for the pod
    - imagePullSecrets: list of imagePullSecrets
    - initContainers: list of initContainers
    - dnsPolicy: dns policy for the pod
    - hostNetwork: host network for the pod
    - hostPID: host PID for the pod
    - hostIPC: host IPC for the pod
    - affinity: affinity for the pod
    - tolerations: list of tolerations
    - priority: priority for the pod
    - priorityClassName: priority class name for the pod
    - nodeSelector: node selector for the pod
    - dnsConfig: dns config for the pod
    - hostname: hostname for the pod
    - subdomain: subdomain for the pod
    - schedulerName: scheduler name for the pod
    - tty: tty for the pod
    - stdin: stdin for the pod
    - stdinOnce: stdin once for the pod
    - terminationGracePeriodSeconds: termination grace period seconds for the pod
    - volumes: list of volumes
    - volumeMounts: list of volume mounts
    - livenessProbe: liveness probe for the pod
    - readinessProbe: readiness probe for the pod
    - startupProbe: startup probe for the pod
    - lifecycle: lifecycle for the pod
  - For a ReplicaSet, Labels are as below - see [ReplicaSet Labels](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#replicaset-v1-apps)
    - replicas: number of replicas
    - selector: selector for the ReplicaSet
    - template: template for the ReplicaSet
    - metadata: metadata for the ReplicaSet
    - spec: spec for the ReplicaSet
    - status: status for the ReplicaSet
  - For a Deployment, Labels are as below - see [Deployment Labels](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#deployment-v1-apps)
    - replicas: number of replicas
    - selector: selector for the deployment
    - template: template for the deployment
    - metadata: metadata for the deployment
    - spec: spec for the deployment
    - status: status for the deployment
  - For a Service, Labels are as below - see [Service Labels](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#service-v1-core)
    - metadata: metadata for the service
    - spec: spec for the service
    - status: status for the service
  - For a PodTemplate, Labels are as below - see [PodTemplate Labels](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#podtemplate-v1-core)
    - metadata: metadata for the PodTemplate
  
## Step-02: Create Simple Pod Definition using YAML

- We are going to create a very basic pod definition
- **02-pod-definition.yml**

```yml
apiVersion: v1 # String
kind: Pod  # String
metadata: # Dictionary
  name: myapp-pod
  labels: # Dictionary 
    app: myapp         
spec:
  containers: # List
    - name: myapp
      image: stacksimplify/kubenginx:1.0.0
      ports:
        - containerPort: 80
```

- **Create Pod**

```bash
# Create Pod
kubectl create -f 02-pod-definition.yml
[or]
kubectl apply -f 02-pod-definition.yml

# List Pods
kubectl get pods
```

## Step-03: Create a LoadBalancer Service
- **03-pod-LoadBalancer-service.yml**
```yml
apiVersion: v1
kind: Service
metadata:
  name: myapp-pod-loadbalancer-service  # Name of the Service
spec:
  type: LoadBalancer
  selector:
  # Loadbalance traffic across Pods matching this label selector
    app: myapp
  # Accept traffic sent to port 80    
  ports: 
    - name: http
      port: 80    # Service Port
      targetPort: 80 # Container Port
```
- **Create LoadBalancer Service for Pod**
```
# Create Service
kubectl apply -f 03-pod-LoadBalancer-service.yml

# List Service
kubectl get svc

# Access Application
http://<Load-Balancer-Service-IP>

```

## API Object References
- [Kubernetes API Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/)
- [Pod Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#pod-v1-core)
- [Service Spec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/#service-v1-core)


