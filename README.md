## Grafana - Docker Metrics on your desktop

---
# Intro
With the introduction of Docker 1.13, docker metrics are now available as a prometheus feed. See https://blog.docker.com/2017/01/whats-new-in-docker-1-13/#h.i7yih1s9513y for more info

# Setup Docker for Mac/Windows
In the Preference/Settings panel, update the daemon to include metrics:
```
{
	"metrics-addr":"0.0.0.0:9323"
}
```
Be careful with the above, as an incorrect setting will prevent docker from starting. 


# Start the Grafana stack with compose
```
docker-compose up -d
```

---
# Manual setup
## <HACK> For Docker share its metrics, with Grafana, an actual prometheus daemon is needed
Add a small listener for the metrics, via prom/prometheus:
```
docker run -d --name prometheus -p 9090:9090 frenchben/prometheus
```
You can verify that the above worked, by visiting http://localhost:9090/metrics - You should see a prometheus-like output.

## Running your Grafana image 
Start your image binding the external port 3000.
```
docker container run -d -p 3000:3000 --name grafana frenchben/grafana
```
Try it out by navigating to http://localhost:3000/dashboard/db/prometheus-stats, default admin user is admin/admin.


_The Docker dashboard was taken from https://grafana.net/dashboards/893, but feel free to add any dashboard you want, it will need some tweaking to work with the new Docker stats__

---
# Build Docker image

```
docker build -t custom-grafana .
docker build -t custom-prometheus -f Dockerfile.prom .
```


