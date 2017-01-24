## Grafana - Docker Metrics on your desktop

---
# Intro
With the introduction of Docker 1.13, docker metrics are now available as a prometheus feed. See https://blog.docker.com/2017/01/whats-new-in-docker-1-13/#h.i7yih1s9513y for more info

# Setup Docker for Mac/Windows
In the Preference/Settings panel, update the daemon to include metrics:
```
{
	"metrics-addr":"0.0.0.0:1337"
}
```
Be careful with the above, as an incorrect setting will prevent docker from starting. 

# <HACK> Setup a listener on Docker for Mac/Windows for that metric
Add a small listener for the metrics:
```
docker container run -it -d --privileged --name slirp-metrics --pid=host debian nsenter -t 1 -m -n /usr/bin/slirp-proxy -i -proto tcp -host-ip 0.0.0.0 -host-port 1336 -container-ip 127.0.0.1 -container-port 1337
```
You can verify that the above worked, by visiting http://localhost:1336/metrics - You should see a prometheus-like output.

# Running your Grafana image 
Start your image binding the external port 3000.
```
docker container run -it -d -p 3000:3000 --name grafana frenchben/grafana
```
Try it out by navigating to http://localhost:3000/dashboard/db/docker-and-system-monitoring, default admin user is admin/admin.


_The dashboard was taken from https://grafana.net/dashboards/893, but feel free to add any dashboard you want_

---
# Build Docker image

```
docker build -t custom-grafana .
```
