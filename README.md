# k8-volumes
k8-volumes


# Volumes
1. Empty Dir
2. Host Path


# Empty Dir
- Create volume at POD level
- emptyDir volume is initially empty when POD is created
- All containers in the pod can read and write the same files in the emptyDir volume, though that volume can be mounted at the same or different paths in each container
- Mount volume to the container inside pods, Volume will be created when POD creates, Volume will be deleted when POD deletes
- Volume is shared between all the containers inside the POD, Volume is stored in the node where POD is running
- Volume is not persistent, data will be lost when POD is deleted
- Use case: Temporary data, cache, scratch space, etc.
```shell
$ kubectl apply -f empty-dir-volume.yaml

Open k9s tool and go to pods.
By selecting the pod `test-empty-dir-volume` and then select the container `almalinux` and then select the shell, you can see the below output.

<<K9s-Shell>> Pod: roboshop/test-empty-dir-volume | Container: almalinux
[root@test-empty-dir-volume /]# cd /mnt/nginx-cache/
[root@test-empty-dir-volume nginx-cache]# ls -la
total 4
drwxrwxrwx. 2 root root  41 Apr 24 10:23 .
drwxr-xr-x. 1 root root  25 Apr 24 10:23 ..
-rw-r--r--. 1 root root   0 Apr 24 10:23 access.log
-rw-r--r--. 1 root root 509 Apr 24 10:23 error.log
[root@test-empty-dir-volume nginx-cache]# cat error.log
2026/04/24 10:23:17 [notice] 1#1: using the "epoll" event method
2026/04/24 10:23:17 [notice] 1#1: nginx/1.29.8
2026/04/24 10:23:17 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2026/04/24 10:23:17 [notice] 1#1: OS: Linux 6.12.79-101.147.amzn2023.x86_64
2026/04/24 10:23:17 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 65536:1048576
2026/04/24 10:23:17 [notice] 1#1: start worker processes
2026/04/24 10:23:17 [notice] 1#1: start worker process 30
2026/04/24 10:23:17 [notice] 1#1: start worker process 31
[root@test-empty-dir-volume nginx-cache]#

```
- Empty dir will in case of side-car containers to read from one container and write to another container.


## Host Path
- A container can access the file system of the node where it is running using hostPath volume.
- Host path volume mounts a file or directory from the host node's filesystem into a pod. 
- This is not that secure to give access to the host file system to the container, so use it with caution.
- But some case, if we want to see the nodes logs we allow pods and container to use hostpath in read only mode.
- Daemonset is used to deploy the pods on all the nodes, so that we can see the logs of all the nodes.
- While worker nodes added, deamonset will automatically deploy the pods on the new nodes and we can see the logs of new nodes as well.
```shell
$ kubectl apply -f 03-host-path-volume.yaml
daemonset.apps/fluentd-elasticsearch created

root@fluentd-elasticsearch-ckrgq:/# cd /var/log
root@fluentd-elasticsearch-ckrgq:/var/log# ls
README  audit           btmp    cloud-init-output.log  containers       dnf.log      hawkey.log  lastlog  private   wtmp
amazon  aws-routed-eni  chrony  cloud-init.log         dnf.librepo.log  dnf.rpm.log  journal     pods     tallylog
root@fluentd-elasticsearch-ckrgq:/var/log#

```