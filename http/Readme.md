# HTTP Routing and Load Balancing

To show the demo, log into the Win2019 jump host as user/user.
Note:  all demo pages are bookmarked in the Chrome browser.
Note:  kubectl is installed on the jump host.  Access through PowerShell.

Once you are logged in, do the following:

1. Find the shortcut on the desktop called “proxy”.  Right click on it and select “Run in PowerShell”.  This will set up a port-forward to the Nginx+ KIC pod on the Kubernetes Master node.  Without it, you won’t be able to perform step 2.

2. Open the Chrome browser and log into the Nginx+ Dashboard (access from bookmark bar).  This will allow you to show how a single instance of the Nginx+ KIC changes as a result of scaling the coffee/tea deployments (step 4).  Navigate to the “HTTP Upstreams” section/tab to see the individual pods that make up each service along with their current connection information.  Note that the KIC is also acting as the Ingress for Prometheus, Grafana and Alert Manager apps in addition to the café app.

3. Open two more browser tabs to access the cafe.example.com web site.  This is a site with two "services", coffee and tea.  In one open tab use the bookmark to open the Coffee service and in the other tab open the Tea service.  This illustrates path-based routing by the KIC.  The KIC is also load balancing amongst the available pods of the coffee and tea services so if you refresh your browser (or set to Auto Refresh), you will notice that you are directed to a different server (pod) each time.  By default, the KIC is using a simple round robin algorithm but this environment is set up to use the Random of Two, least time algorithm.  This is only available with NGINX+.

4. Open a PoweerShell terminal.  With the dashboard visible, scale the coffee and tea deployments and see the changes in the Nginx+ dashboard:

`kubectl scale deployment -n cafe tea –-replicas=4`

`kubectl scale deployment -n cafe coffee –-replicas=4`

The Nginx+ KIC is recognizing changes to the services it is load balancing for and is changing all the Nginx+ instances dynamically and without dropping existing connections.

5. In general, the Ingress Controller is configured with Ingress resources.  F5/NGINX has created custom resources to give easier access to more of the functionality of NGINX+.  In this environment, we have used the VirtualServer custom resource to configure the NGINX load balancer pods.  You can find an example of the custom resource here: https://github.com/nginxinc/kubernetes-ingress/blob/v2.0.3/examples-of-custom-resources/basic-configuration/cafe-virtual-server.yaml 

6. Open another browser tab.  Access Prometheus Metrics page using the bookmark.  Type “nginx” into the search box to show all the Nginx specific metrics that are being captured.  These metrics can be used in the AlertManager and displayed in Grafana.

7. Open another browser tab.  Access the Nginx+ Grafana Dashboard page via the bookmark.  The dashboard shows aggregated metrics from all the KIC pods in the cluster.

8. To show App Protect working, access the NAP bookmark.  You will see that the request has been blocked with the standard NAP response page due to a XSS violation.  