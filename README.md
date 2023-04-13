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
 
