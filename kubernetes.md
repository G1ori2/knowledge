# Daemonset

## uses of Daemonset are (on every node) : 

- Cluster storage
- logs collection
- node monitoring

## yaml ? 

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
 # satisfy “DNS Subdomain Names”
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      # these tolerations are to have the daemonset runnable on control plane nodes
      # remove them if your control plane nodes should not run pods
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log

```

# Replication Controller

## what ?

ensure that a **specified** number of pod replicas are running at any one time.

In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is **always** up and available.

**Automatically**

- Too many

​		terminates the extra pods

- Too few

​		starts more pods

Your pods are re-created on a node after disruptive maintenance such as a kernel upgrade. 

**For this reason,** you should use a ReplicationController **even if** your application requires only **a single pod.** A ReplicationController is similar to a process supervisor, but instead of supervising individual processes on a single node, the ReplicationController supervises multiple pods across multiple nodes.



## yaml

### rc config

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80

```

