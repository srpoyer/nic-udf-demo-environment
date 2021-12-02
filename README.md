# nic-udf-demo-environment

Demo instructions and diagram for UDF demo environment

This NGINX+ KIC Blueprint lets you demonstrate the Nginx+ Kubernetes Ingress Controller (KIC) using basic applications deployed in the cluster.  The environment also lets you look at metrics in both the built-in Nginx+ console as well as in Grafana.  There is a traffic generator to make your dashboards look more interesting.

If you need more information about why you need an Ingress Controller, watch this video:  https://youtu.be/VicH6KojwCI.

The components of the environment include:

- 3 node K8s cluster, one Control Plane (aka master) and two workers.  Each node has a KIC pod since the KIC is deployed as a DaemonSet.  The cluster is also running two applications as K8s Deployments, namely, coffee and tea.  These are simply web servers that will return web pages with some identifying information about the pods they run in.
- front-end Nginx+ load balancer that all requests will go to and which will distribute load to the KIC pods.
- Windows jump host to show the demo from.  You will use the Chrome browser to show various web pages and PowerShell to scale the coffee and tea apps with the `kubectl` command.

Here are the currently available demo scenarios:

- [L7/HTTP Routing and Load Balancing](http/Readme.md)

- [L4/TCP & UDP Load Balancing](tcp-udp/Readme.md)



