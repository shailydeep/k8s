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
############### 


