# docker-no-space-solution
The problem is that `/var/lib/docker` is on the `/` filesystem, which is running out of inodes. You can check this by running:
```
df -i /var/lib/docker
```

Since `/home` filesystem has sufficient inodes and disk space, moving Docker's working directory there, there should get it going again.

* There is nothing valuable in the current Docker install.

First stop the Docker daemon. On Ubuntu, run

```
sudo service docker stop
```
Then move the old `/var/lib/docker` out of the way:
```
sudo mv /var/lib/docker /var/lib/docker~
```
Now create a directory on `/home`:
```
sudo mkdir /home/docker
```
and set the required permissions:
```
sudo chmod 0711 /home/docker
```
Link the `/var/lib/docker` directory to the new working directory:
```
sudo ln -s /home/docker /var/lib/docker
```
Then restart the Docker daemon:
```
sudo service docker start
```
Done. It should work now without any issues.
