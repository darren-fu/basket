
启动rancher agent 失败:
```
Add explanation to Agent FAQ when following log message is seen:

ERROR: DNS Checking loopback IP address 127.0.0.0/8, localhost or ::1 configured as the DNS server on the host file /etc/resolv.conf, can't accept it

https://docs.docker.com/engine/installation/linux/linux-postinstall/#dns-resolver-found-in-resolvconf-and-containers-cant-use-it
```
#### Specify DNS servers for Docker

1. Create or edit the Docker daemon configuration file, which defaults to /etc/docker/daemon.json file, which controls the Docker daemon configuration.
```
$ sudo nano /etc/docker/daemon.json
``` 
2. Add a dns key with one or more IP addresses as values. If the file has existing contents, you only need to add or edit the dns line.
```
{
	"dns": ["8.8.8.8", "8.8.4.4"]
}
```
If your internal DNS server cannot resolve public IP addresses, include at least one DNS server which can, so that you can connect to Docker Hub and so that your containers can resolve internet domain names.

Save and close the file.

3. Restart the Docker daemon.
```
$ sudo service docker restart
```

4. Verify that Docker can resolve external IP addresses by trying to pull an image:
``` 
$ docker pull hello-world
```
5. If necessary, verify that Docker containers can resolve an internal hostname by pinging it.
```
$ docker run --rm -it alpine ping -c4 <my_internal_host>

PING google.com (192.168.1.2): 56 data bytes
64 bytes from 192.168.1.2: seq=0 ttl=41 time=7.597 ms
64 bytes from 192.168.1.2: seq=1 ttl=41 time=7.635 ms
64 bytes from 192.168.1.2: seq=2 ttl=41 time=7.660 ms
64 bytes from 192.168.1.2: seq=3 ttl=41 time=7.677 ms
```
