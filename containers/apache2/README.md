
## Create a volume in Longhorn
 * Attach volume to a longhorn node
 * List attached volumes on longhorn node to get the device : /dev/sd[*]
 * Format the attached device w/ EXT4
   ```console
   sudo mkfs -t ext4 /dev/sdb
   ```
 * Make directory to mount
   ```console
   sudo mkdir -p /tmp/folder
   ```
 * Mount the device to the newly created directory
   ```console
   sudo mount /dev/sdb /tmp/folder
   ```
   ```console
   sudo mkdir -p /tmp/folder/apache2_configs
   ```
   ```console
   sudo mkdir -p /tmp/folder/apache2_logs
   ```
 * Copy apache2.conf file to /tmp/folder/apache2_configs
   ```console
   sudo rsync -avxHAX lorenzo@192.168.10.74:/home/lorenzo/dns-container-b9/apache2-etc/* /tmp/folder/apache2_configs/
   ```
 * Umount /tmp/folder
   ```console
   sudo umount /tmp/folder
   ```
 * Go to Longhorn and detach the volume and leave it in an detached state as it will be autoamtically mounted whenever the manifest (apache2_deployment.yaml) is applied via kubectl
 * Create a PV/PVC on that volume in Longhorn.  Be mindful and ensure you add the correct namespace (namespace must already exist)
 ![image](https://github.com/dcodev1702/k3s_cluster_rancher_longhorn/assets/32214072/b11a06c5-3717-4e1e-abd1-5b72385a9189)

 * Provision the containerized workload!
   ```console
   kubectl apply -f ./apache2_deployment.yaml
   ```

Helpful Commands: <br />
```console
kubectl get pods -n bind9-logstash
```
```console
kubectl exec -it -n bind9-logstash apache2-web-85d5479df-ftgw6 /bin/bas
```
```console
kubectl delete -f ./apache2_deployment.yaml
```
```console
kubectl -n bind9-logstash describe pod apache2-web
```
