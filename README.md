# k8s
###########COMMANDS FOR KUBERNETES###############
*********commands for pod **************
kubectl get pod
kubectl apply -f file_name.yml 
kubectl delete pod pod_name
kubectl get pods -o wide
kubectl describe pod pod_name
kubectl logs -f pod_name
kubectl logs -f pod -c container_name
kubectl exec multictrpod -it -c c002 -- /bin/bash 
*********labels selector********************
 kubectl get pod --show-labels
 kubectl labels pods labelspod myname=shily
 kubectl get pods -l class=pods
 kubectl label pod testpod4 address=aligarh
 kubectl get pods -l class!=pod
 kubectl get pods -l 'env in (development)'
 kubectl get pods -l 'env notin (devlopment,shaily)'
 kubectl get pods -l env=devlopment,class=pod
 kubectl get pods -l env!=devlopment,class!=pods 
 kubectl delete pods -l 'env in(devlopment)'
 ****************NodeLabels******************
kubectl get nodes
 kubectl apply -f nodelabels.yml
 kubectl describe pod nodelabels
 kubectl label nodes minikube hardware=t2-medium
 kubectl delete node minikube
 ************Repication-Controller***************
 kubectl apply -f replicasetPod.yml
 kubectl get rc
 kubectl get pod
 kubectl describe rc myreplica
 kubectl delete pod myreplica-cp7r7
 kubectl get pods --show-labels
  kubectl scale --replicas=2 rc -l myname=shailythaku
  kubectl scale --replicas=5 rc -l myname=shailythakur
  ******************ReplicaSet*****************
  kubectl apply -f replicaset.yml
   kubectl describe rs myr
   kubectl get rs
   ***************** Deployments****************
   kubectl apply -f deployment.yml
   kubectl get deploy  #to check deployments
    kubectl get pod
kubectl describe deploy mydeployment ######## more information about deployment
kubectl get rs
kubectl scale --replicas=1 deploy mydeployment  #### to scal up and down the replicaset 
kubectl scale --replicas=5 deploy mydeployments
kubectl logs -f mydeployments-74b5887dc8-922rl   #### to check the what is running inside container
kubectl exec mydeployments-7dcc44d4bf-7jqzt -- cat /etc/os-release  ######## to check what is the image and server inside the container
kubectl rollout status deployment mydeployments   ###to check the status of depoyment
kubectl rollout history deployment mydeployments
kubectl rollout undo deploy/mydeployments
kubectl rollout undo deploy/mydeployments
################ multicontainer in a pod ############################
kubectl apply -f 2cntr2pod.yml
kubectl get pods
kubectl exec testpod -it -c ctr1 -- /bin/bash
now install the curl application to access the the other container
then you can access other container by ysing localhost
curl localhost:80
###############multi-pod-container communication###############
kubectl apply -f multupod.yml
kubectl aaply -f multipod2.yml
kubectl get pods
now both containers are running
then we can access container by using the pod ip addrresses
curl 172.17.0.2:80
#############Service-object##############
vi servicedeploy.yml
kubectl apply -f servicedeploy.yml
vi service.yml
kubectl apply -f service.yml
now you can access pod by using the service virtual ip
kubectl get pod -o wide
is will show the service virtual ip
then,
curl v-ip
vi hostpath.yml
kubectl apply -f hostpath.yml
kubectl get pods
kubectl exec myvolhostpath -it -c testc -- /bin/bash   ### go inside the container of pod 
then create some files inside the container
then exit
now you can see those created file on you hostmachine
############# Persistent-Volume/PErsistentVolumeClaim##############3
Before you begin
You need to have a Kubernetes cluster that has only one Node, and the kubectl command-line tool must be configured to communicate with your cluster. If you do not already have a single-node cluster, you can create one by using Minikube.

Familiarize yourself with the material in Persistent Volumes.
Create an index.html file on your Node
Open a shell to the single Node in your cluster. How you open a shell depends on how you set up your cluster. For example, if you are using Minikube, you can open a shell to your Node by entering minikube ssh.

In your shell on that Node, create a /mnt/data directory:


In the /mnt/data directory, create an index.html file:
# This again assumes that your Node uses "sudo" to run commands
# as the superuser
sudo sh -c "echo 'Hello from Kubernetes storage' > /mnt/data/index.html"
Create a PersistentVolume
In this exercise, you create a hostPath PersistentVolume. Kubernetes supports hostPath for development and testing on a single-node cluster. A hostPath PersistentVolume uses a file or directory on the Node to emulate network-attached storage.

In a production cluster, you would not use hostPath. Instead a cluster administrator would provision a network resource like a Google Compute Engine persistent disk, an NFS share, or an Amazon Elastic Block Store volume. Cluster administrators can also use StorageClasses to set up dynamic provisioning.
The configuration file specifies that the volume is at /mnt/data on the cluster's Node. The configuration also specifies a size of 10 gibibytes and an access mode of ReadWriteOnce, which means the volume can be mounted as read-write by a single Node. It defines the StorageClass name manual for the PersistentVolume, which will be used to bind PersistentVolumeClaim requests to this PersistentVolume.

Create the PersistentVolume:
vi persistentvolume.yml
kubectl apply -f persistentvolume.yml
kubectl get pv
The output shows that the PersistentVolume has a STATUS of Available. This means it has not yet been bound to a PersistentVolumeClaim.
Create a PersistentVolumeClaim
The next step is to create a PersistentVolumeClaim. Pods use PersistentVolumeClaims to request physical storage. In this exercise, you create a PersistentVolumeClaim that requests a volume of at least three gibibytes that can provide read-write access for at least one Node.

Here is the configuration file for the PersistentVolumeClaim:
vi  pcClaim-volume.yml
kubectl apply -f pcClaim.ym
kubectl get pvc
Now the output shows a STATUS of Bound.Now the output shows a STATUS of Bound.
Create a Pod / you can also create deployment for pod replicSET
The next step is to create a Pod that uses your PersistentVolumeClaim as a volume.

Here is the configuration file for the Pod:
Notice that the Pod's configuration file specifies a PersistentVolumeClaim, but it does not specify a PersistentVolume. From the Pod's point of view, the claim is a volume.

Create the Pod:
Get a shell to the container running in your Pod:
kubectl exec -it task-pv-pod -- /bin/bash
ls
cd /mnt
apt update
apt install curl
curl http://localhost/
The output shows the text that you wrote to the index.html file on the hostPath volume:

Hello from Kubernetes storage

If you see that message, you have successfully configured a Pod to use storage from a PersistentVolumeClaim.

Clean up
Delete the Pod, the PersistentVolumeClaim and the PersistentVolume:

kubectl delete pod task-pv-pod
kubectl delete pvc task-pv-claim
kubectl delete pv task-pv-volume

############## LivenessProbe#################
vi livenessprobe.yml
kubectl apply -f livenessprobe.yml
kubectl get pod
kubectl exec pod_name -it -- /bin/bash
now toy have created file inside the container /tmp/healthy
cat /tmp/healthy   #### fi this command is successful then is return 0
0   ### for status pass
1   ### error
2 #### error
exit/terminate the container it return code error 137
to check the status command is 
echo $?


#########ConfigMap#################
# by volume configmap 
# first you need to create config file for configmap
vi sampl.conf
# it is a configuration file
kubectl create configmap cnf_name --from-file=sampl.yml
# now crate a pod 
vi deployconfig.yml
kubectl apply -f deployconfig.yml
kubectl get pod
kubectl exec pod_name -it -- /bin/bash  # go inside the container 
cd /tmp/config
ls
sample.conf
# by environment variable configmap
vi cnfenvpod.yml
kubectl apply -f cnfenvpod.yml
kubectl get pods
kubectl exec pod_name -it -- /bin/bash
env     # now you can see create configfile in env format

############secrets#########################
now you need to creare two file you define the data like username and password

 echo "root" > username.txt; echo "mypassword" > password.txt
 # create secret 
 kubectl create secret generic mysecret --from-file=username.txt --from-file=password.txt

kubectl get secret
kubectl describe secret mysecret
output like 
username 16 bytes
password 5 bytes
# now create a pod 
vi secrets.yml
kubectl apply -f secrets.yml
# go inside the container and check your data only container can access the data in secrets other person can not


kubectl exec mysecret -it -- /bin/bash

############Namespace#######################
# create namespace 
vi namespace.yml
 kubectl apply -f namespacepod.yml -n dev
 kubectl get pod -n dev 
kubectl config set-context $(kubectl config current-context) --namespace=dev   # this command is used to set the namespace so k8s by default found resources in created namespace
kubectl get pod 
kubectl config view | grep namespace:      # show the namespace name
kubectl config view    # more information about the namespace
kubectl get pods -n default      # if we have set the above context then it is found resource in default namespace using -n flag...

########Reasource Quata #######################33

A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per namespace. It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that namespace.
creating files 
then create pods
now check information of the and check the pod is created or not
kubectl describe pod pod_name


############## Horizontal Pod Autoscaler ###################################

first of all you need install the metric server for horizontal autoscaller 
install:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl top nodes
kubectl get deployment metrics-server -n kube-system
# enavle the metric server
minikube addons enable metrics-server
kubectl get namespaces    # get the namespace of kube-system 
kubectl get pods -n kube-system
vi HPAdeployment.yml
kubectl apply -f HPAdeployment.yml
kubectl autoscale deployment mydeploy --cpu-percent=20 --min=1 --max=1     # set the horizontal autoscale pod
kubectl get all
watch kubectl get all       # see live autoscaling of the pod
# take a new terminal for loading the machine
then
# go inside the container and load..........
kubectl exec pod_name -it -- /bin/bash
apt update


