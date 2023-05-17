# Replica set
### Write a common use-case, where you will use a daemon set instead of replica set?
- We can use demon set in log level, when we need to collect logs from each Nodes.

# Deployment
### Suppose you have deployed your application using a deployment controller. Assume the initial number of replicas is one. Write the steps needed to update a container's image using deployment, making sure that there is zero downtime.
- Build the new image and update the deployment configuration with the changes you need with the below command
  ```
    vim kubia-deployment-and-service-v1.yaml 
    ```
     *lets change the replicaset to 1 and image version to v2*

- Then apply the update deployment file using 
   ``` 
    kubectl apply -f kubia-deployment-and-service-v1.yaml
    ```
- Now the rolling out process id going on
 - Once the new pod fully running the older one will go terminating 
 - You can check using
 ``` 
kubectl get po
```
    NAME                      READY   STATUS        RESTARTS   AGE
    db-b54cd94f4-t8vwt        1/1     Running       0          19h
    kubia-586b45dbdc-pvxpz    1/1     Terminating   0          15m
    kubia-7d5c456ffc-vzw2b    1/1     Running       0          3s
    redis-868d64d78-lw2r6     1/1     Running       0          21h
    result-5d57b59f4b-vp7rl   1/1     Running       4          21h
    vote-94849dc97-xq5rt      1/1     Running       0          21h
    worker-dd46d7584-l2tnd    1/1     Running       1          19h

# Service
### You have deployed an application, that is listening at port 8000. You used a replica-set to deploy it and created a NodePort service to make it accessible. But when you test it, somehow the application is not reachable at the port. Write down your approach and sequentially all the steps that you will take to find out the issue.
- use ``` kubectl get deploy ``` command to check if the deployment is running or not
- check the desired number of replica set is created using ``` kubectl get rs ```
- check pods are runnig or not using ``` kubectl get po ```
- you can aslo check the service status of nodeport using ``` kubectl get svc ```
- Check decribe service of that nodeport using ``` kubectl describe service <nodeport name> ``` , see the Endpoints and nodeport 
- also check endpoints using ``` kubectl get ep ``` 

```
[root@ip-172-31-29-124 05-services]# kubectl describe service kubia-nodeport
Name:                     kubia-nodeport
Namespace:                kaly
Labels:                   <none>
Annotations:              <none>
Selector:                 app=kubia
Type:                     NodePort
IP Families:              <none>
IP:                       10.98.71.30
IPs:                      <none>
Port:                     <unset>  80/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  30123/TCP
Endpoints:                192.168.45.134:8080,192.168.45.140:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
[root@ip-172-31-29-124 05-services]# kubectl get ep
NAME             ENDPOINTS                                 AGE
db               192.168.45.188:5432                       21h
kubia            192.168.45.134:8080,192.168.45.140:8080   60m
kubia-nodeport   192.168.45.134:8080,192.168.45.140:8080   21m
redis            192.168.45.161:6379                       21h
result           192.168.45.159:80                         21h
vote             192.168.45.160:80                         21h
[root@ip-172-31-29-124 05-services]#
