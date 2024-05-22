--------------- Manual Scheduling ----------------<br>
<br>
A pod definition file nginx.yaml is given. Create a pod using the file.
<pre>kubectl apply -f nginx.yaml</pre>

Manually schedule the pod on node01.
<pre> kubectl delete po nginx</pre>

<pre>
vi nginx.yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeName: node01
  containers:
  -  image: nginx
     name: nginx
</pre>
<pre>kubectl apply -f nginx.yaml</pre>

---------- Labels and Selectors ---------------<br>
<br>
deployed a number of PODs. They are labelled with tier, env and bu. How many PODs exist in the dev environment (env)
<pre>kubectl get pod --selector env=dev</pre>

How many PODs are in the finance business unit (bu)?
<pre>kubectl get pod --selector bu=finance</pre>

How many pods, replica present
<pre>kubectl get all --selector env=prod</pre>

Get pod with selector
<pre>kubectl get pod --selector bu=finance -l tier=frontend</pre>

---------- Taints and Tolerations ------------<br>
<br>
Do any taints exist on node01 node?
<pre>kubectl describe node node01</pre>
or
<pre>kubectl describe node node01 | grep -i taints</pre>

Create a taint on node01 with key of spray, value of mortein and effect of NoSchedule
<pre>kubectl taint nodes node01 spray=mortein:NoSchedule</pre>

Create another pod named bee with the nginx image, which has a toleration set to the taint mortein.
<pre>
apiVersion: v1
kind: Pod
metadata:
  name: bee
spec:
  containers:
  - image: nginx
    name: bee
  tolerations:
  - key: spray
    value: mortein
    effect: NoSchedule
    operator: Equal
</pre>
<pre>kubectl create -f <FILE-NAME>.yaml </pre>

Remove the taint on controlplane, which currently has the taint effect of NoSchedule.
<pre>kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-</pre>

----------- Node Affinity ------------<br>
<br>
How many Labels exist on node node01?
<pre>kubectl describe node node01</pre>

Apply a label color=blue to node node01
<pre>kubectl label node node01 color=blue</pre>

Set Node Affinity to the deployment to place the pods on node01 only.
<pre>Update the deployment by running kubectl edit deployment blue and add the nodeaffinity section as follows:
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
</pre>

Which nodes are the pods placed on now?
<pre>kubectl get pods -o wide</pre>

Create a new deployment named red with the nginx image and 2 replicas, and ensure it gets placed on the controlplane node only.
Use the label key - node-role.kubernetes.io/control-plane - which is already set on the controlplane node.
<pre>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/control-plane
                operator: Exists
</pre>

<pre>kubectl create -f deploy.yaml</pre>

