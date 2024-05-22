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
