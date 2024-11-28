```
Usage of ./kubeexporter:
  -auth string
    	Path to the auth file
  -check-interval int
    	Interval in seconds to check for URL file changes (default 5)
  -enable-core-metrics
    	Enable or disable core Kubernetes metrics collection (default true)
  -metrics-check-interval int
    	Interval in seconds to check metrics-server status (default 5)
  -port int
    	Port to expose the metrics (default 8089)
  -timeout int
    	Timeout in milliseconds for fetching metrics (default 100)
  -url string
    	Path to the URL file
  -version
    	Print version information and exit
```
```
How to use
1. kubectl apply -f deployment.yaml
2. kubectl get pods --all-namespaces

you will get beolw pods
kube-state-metrics
metrics-server

3. kubectl get svc -A
you will get CLUSTER-IP and PORT for
kube-state-metrics
coredns

4. vim /usr/local/bin/url.txt
http://CLUSTER-IP:PORT kube-state
http://CLUSTER-IP:PORT coredns

5. vim /usr/local/bin/auth.txt
username:password

6. vim /etc/systemd/system/kubeexporter.service
[Unit]
Description=KubeExporter Service
After=network.target

[Service]
ExecStart=/usr/local/bin/kubeexporter_amd64 -auth /usr/local/bin/auth.txt -port 8089 -url /usr/local/bin/url.txt -timeout 300 -enable-core-metrics=false
Restart=always
User=root
Group=root

[Install]
WantedBy=multi-user.target

7. systemctl start kubeexporter
```
