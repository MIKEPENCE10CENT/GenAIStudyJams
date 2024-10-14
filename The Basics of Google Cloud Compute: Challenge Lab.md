### Create a Disk

```bash
gcloud compute disks create mydisk --size=200GB \
--zone $ZONE
```

### Attach Disk to Instance
```
gcloud compute instances attach-disk my-instance \
--disk mydisk \
--zone $ZONE
```
### TASK 3: Install and Start NGINX
Update the package lists:
```
sudo apt update
```
Install NGINX:
```
sudo apt install nginx -y
```
Start the NGINX service:
```
sudo systemctl start nginx
```
