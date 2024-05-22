# kuberenets-lab-commands

------------------ PODS -----------------------<br>
<br>
Create a new pod with the nginx image.
<pre>kubectl run nginx --image=nginx</pre>

To check pods
<pre>kubeclt get pods</pre>

To describe pod - which image id its using, Which nodes are these pods placed on, how many container are running on the pods
<pre>kubectl describe pod newpods-nmsxl</pre>

Now change the image on this pod to redis. Make pod status to running state.
<pre>kubectl edit pod redis - edit the name of redis123 to redis</pre>
<pre>kubeclt get pods</pre>

If you used a pod definition file then update the image from redis123 to redis and then run kubectl apply command to update the image :-
<pre>kubectl apply -f redis-definition.yaml </pre>

To delete pod
<pre>kubectl delete pod webapp</pre>

------------------ REPLICA SETS -----------------------<br>
<br>
How many ReplicaSets exist on the system?
<pre>kubectl get replicasets</pre>

Create a ReplicaSet using the replicaset-definition-1.yaml file located at /root/.
There is an issue with the file, so try to fix it.
<pre> kubectl api-resources | grep replicaset</pre>
<pre>kubectl explain replicaset | grep VERSION </pre>
<pre> vi replicaset-definition-1.yaml - update the version </pre>
<pre>kubectl create -f /root/replicaset-definition-1.yaml</pre>

Fix the original replica set new-replica-set to use the correct busybox image.
<pre>
  kubectl edit replicaset new-replica-set
  kubectl get pods
  Delete the previous pods to get the new ones with the correct image. For this, run the command: kubectl delete po <pod-name>
</pre>

Scale the ReplicaSet to 5 PODs.
<pre> kubectl edit replicaset new-replica-set
   OR
kubectl scale rs new-replica-set --replicas=5 </pre>
  
Delete any one of the 4 PODs.
<pre>kubectl delete pod <name-of-the-pod> </pre>

------------------ DEPLOYMENT -----------------------<br>
<br>
To check deployment
<pre> kubectl get deployment </pre>
Edit and apply deployment 
<pre> vi deployment-definition-1.yaml </pre>
<pre>kubectl apply -f deployment-definition-1.yaml</pre>

Create a new deployment- name=httpd-frontend, image=httpd:2.4-alpine, replicas=3
<pre>
  ---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      name: httpd-frontend
  template:
    metadata:
      labels:
        name: httpd-frontend
    spec:
      containers:
      - name: httpd-frontend
        image: httpd:2.4-alpine
</pre>
------------------ NAMESPACES ----------------------<br>
<br>
To get namespaces
<pre>kubectl get ns</pre>

To check how many pods present in particular namespace
<pre>kubectl get pods -n research</pre>

To create namespace
<pre>kubectl create ns test</pre>
Create a POD in the finance namespace. Name=redis , image=redis
<pre>kubectl run redis --image=redis -n finance</pre>

------------------ SERVICES----------------------<br>
<br>
To get service which are available
<pre>kubectl get svc </pre>

To know the target port
<pre>kubectl describe svc</pre>
Create a new service to access the web application using the service-definition-1.yaml file.<br>
Name: webapp-service<br>
Type: NodePort<br>
targetPort: 8080<br>
port: 8080<br>
nodePort: 30080<br>
selector:<br>
  name: simple-webapp<br>
<pre>vi service-defintion-1.yaml</pre>
<pre>kubectl apply -f /root/service-definition-1.yaml</pre>

--------------- Imperative Commands --------------<br>
<br>
Deploy a pod named nginx-pod using the nginx:alpine image.
<pre>kubectl run nginx-pod --image=nginx:alpine</pre>

Deploy a redis pod using the redis:alpine image with the labels set to tier=db.
<pre>kubectl run redis --image=redis:alpine -l tier=db</pre>

Create a service redis-service to expose the redis application within the cluster on port 6379.
<pre>kubectl expose pod redis --port=6379 --name=redis-service</pre>

Create a deployment named webapp using the image kodekloud/webapp-color with 3 replicas.
<pre>kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3</pre>

Create a new pod called custom-nginx using the nginx image and expose it on container port 8080.
<pre>kubectl run custom-nginx --image=nginx --port=8080</pre>

Create a new namespace called dev-ns.
<pre>kubectl create ns dev-ns</pre>

Create a new deployment called redis-deploy in the dev-ns namespace with the redis image. It should have 2 replicas.
<pre>kubectl create deployment redis-deploy --image=redis --replicas=2 -n dev-ns</pre>

Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the
same name (httpd). The target port for the service should be 80.
<pre>kubectl run httpd --image=httpd:alpine --port=80 --expose</pre>
