## Voting App ##

###   Write your observations in a text file.?

	 - delete voter pod -> the pod will recreated - container may change,  (cause of replica set)
	 - delete worker pod -> the pod will recreated - container may change,  (cause of replica set)
	 - delete DB pod-> Data has been lost, the pod will recreated - container may change,  (cause of replica set)
		
	
###   what happens after db pod deletion?
         - Data has been lost
 	 - Db created again config file, templates, aubdirectories
         - Worker node contains tha data even if the db deleted
         - The worker pod and Result pod will be restarted.

###    Write in the text file, what you did in order to make the result pod work.
         - Nothing, The Result pod will work as soon as the db pod created again

``` [root@ip-172-31-29-124 example-voting-app]# kubectl get po
NAME                      READY   STATUS        RESTARTS   AGE
db-b54cd94f4-h8n6n        1/1     Running       0          27s
db-b54cd94f4-td9pf        1/1     Terminating   0          33m
redis-868d64d78-lw2r6     1/1     Running       0          41m
result-5d57b59f4b-vp7rl   1/1     Running       2          41m
vote-94849dc97-xq5rt      1/1     Running       0          41m
worker-dd46d7584-8mcdk    1/1     Running       2          41m
```

***Commands Used***

kubectl get po  => 
	NAME                      READY   STATUS    RESTARTS   AGE
	db-b54cd94f4-h8n6n        1/1     Running   0          58m
	redis-868d64d78-lw2r6     1/1     Running   0          98m
	result-5d57b59f4b-vp7rl   1/1     Running   3          98m
	vote-94849dc97-xq5rt      1/1     Running   0          98m
	worker-dd46d7584-8mcdk    1/1     Running   3          98m

kubectl delete po worker-dd46d7584-8mcdk =>
	pod "worker-dd46d7584-8mcdk" deleted

kubectl logs db-b54cd94f4-h8n6n  => See logs of that pod
