# nic-udf-demo-environment
Demo instructions and diagram for UDF demo environment

This NGINX+ KIC Blueprint lets you demonstrate the Nginx+ Kubernetes Ingress Controller (KIC) using basic applications deployed in the cluster.  The environment also lets you look at metrics in both the built-in Nginx+ console as well as in Grafana.  There is a traffic generator to make your dashboards look more interesting.

If you need more information about why you need an Ingress Controller, watch this video:  https://youtu.be/VicH6KojwCI.

The components of the environment include:

- 3 node K8s cluster, one Control Plane (aka master) and two workers.  Each node has a KIC pod since the KIC is deployed as a DaemonSet.  The cluster is also running two applications as K8s Deployments, namely, coffee and tea.  These are simply web servers that will return web pages.
- front-end Nginx+ load balancer that all requests will go to and which will distribute load to the KIC pods.
- Windows jump host to show the demo from.  You will use the Chrome browser to show various web pages and PowerShell to scale the coffee and tea apps.

Here are the currently available demo scenarios:
- L7/HTTP Routing and Load Balancing
- L4/TCP & UDP Load Balancing

To show the demo, log into the Win2019 jump host as user/user.
Note:  all demo pages are bookmarked in the Chrome browser.
Note:  kubectl is installed on the jump host.  Access through PowerShell.
Once you are logged in, do the following:

Find the shortcut on the desktop called “proxy”.  Right click on it and select “Run in PowerShell”.  This will set up a port-forward to the Nginx+ KIC pod on the Kubernetes Master node.  Without it, you won’t be able to perform step 2.
Open the Chrome browser and log into the Nginx+ Dashboard (access from bookmark bar).  This will allow you to show how a single instance of the Nginx+ KIC changes as a result of scaling the coffee/tea deployments (step 4).  Navigate to the “HTTP Upstreams” section/tab to see the individual pods that make up each service along with their current connection information.  Note that the KIC is also acting as the Ingress for Prometheus, Grafana and Alert Manager apps in addition to the café app.
Open two more browser tabs to access the cafe.example.com web site.  This is a site with two "services", coffee and tea.  In one open tab use the bookmark to open the Coffee service and in the other tab open the Tea service.  This illustrates path-based routing by the KIC.  The KIC is also load balancing amongst the available pods of the coffee and tea services so if you refresh your browser (or set to Auto Refresh), you will notice that you are directed to a different server (pod) each time.  By default, the KIC is using a simple round robin algorithm but this environment is set up to use the Random of Two, least time algorithm.  This is only available with NGINX+.
Open a PoweerShell terminal.  With the dashboard visible, scale the coffee and tea deployments and see the changes in the Nginx+ dashboard:
kubectl scale deployment -n cafe tea –-replicas=4
kubectl scale deployment -n cafe coffee –-replicas=4
The Nginx+ KIC is recognizing changes to the services it is load balancing for and is changing all the Nginx+ instances dynamically and without dropping existing connections.
5.	Open another browser tab.  Access Prometheus Metrics page using the bookmark.  Type “nginx” into the search box to show all the Nginx specific metrics that are being captured.  These metrics can be used in the AlertManager and displayed in Grafana.
6.	Open another browser tab.   Access the Nginx+ Grafana Dashboard page via the bookmark.    The dashboard shows aggregated metrics from all the KIC pods in the cluster.