------- STATIC PODS ----------<br>
<br>
How many static pods exist in this cluster in all namespaces?
<pre>kubectl get pods --all-namespaces and check for control plane append</pre>

Create a static pod named static-busybox that uses the busybox image and the command sleep 1000
<pre>Create a pod definition file in the manifests folder. To do this, run the command:
kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml</pre>

Edit the image on the static pod to use busybox:1.28.4
<pre>Simply edit the static pod definition file and save it. If that does not re-create the pod, run: kubectl run --restart=Never --image=busybox:1.28.4 static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml</pre>

We just created a new static pod named static-greenbox. Find it and delete it.
This question is a bit tricky. But if you use the knowledge you gained in the previous questions in this lab, you should be able to find the answer to it.
<pre>
  First, let's identify the node in which the pod called static-greenbox is created. To do this, run:
    root@controlplane:~# kubectl get pods --all-namespaces -o wide  | grep static-greenbox
      default       static-greenbox-node01                 1/1     Running   0          19s     10.244.1.2   node01       <none>           <none>
    root@controlplane:~#
      From the result of this command, we can see that the pod is running on node01.

  Next, SSH to node01 and identify the path configured for static pods in this node.
  Important: The path need not be /etc/kubernetes/manifests. Make sure to check the path configured in the kubelet configuration file.
  root@controlplane:~# ssh node01 
  root@node01:~# ps -ef |  grep /usr/bin/kubelet 
    root        4147       1  0 14:05 ?        00:00:00 /usr/bin/kubelet --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf --config=/var/lib/kubelet/config.yaml --container-runtime-endpoint=unix:///var/run/containerd/containerd.sock --pod-infra-container-image=registry.k8s.io/pause:3.9
    root        4773    4733  0 14:05 pts/0    00:00:00 grep /usr/bin/kubelet
  root@node01:~# grep -i staticpod /var/lib/kubelet/config.yaml
  staticPodPath: /etc/just-to-mess-with-you
  root@node01:~# 
  Here the staticPodPath is /etc/just-to-mess-with-you
  Navigate to this directory and delete the YAML file:
  root@node01:/etc/just-to-mess-with-you# ls
    greenbox.yaml
  root@node01:/etc/just-to-mess-with-you# rm -rf greenbox.yaml 
  root@node01:/etc/just-to-mess-with-you#
  Exit out of node01 using CTRL + D or type exit. You should return to the controlplane node. Check if the static-greenbox pod has been deleted:
  root@controlplane:~# kubectl get pods --all-namespaces -o wide  | grep static-greenbox
  root@controlplane:~# 
</pre>
