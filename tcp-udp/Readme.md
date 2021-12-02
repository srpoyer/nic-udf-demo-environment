# TCP-UDP Load Balancing

In this demo you can show TCP and UDP load balaning.  The environment has an extra CoreDNS deployment running in the "core" namespace.  You can use `dig` to generate DNS requests using both TCP and UDP.  

Note:  This demo is run from a terminal so putty or iTerm is required.

Here is what the setup looks like:

![diagram](UDF%20TCP%20&%20UDP.jpeg)

Here are the steps:

1. Open up a shell to the "external-lb" component using the SSH access method.  
2. Run the following command, showing an example UDP request:

```bash
dig @10.1.1.8 -p 5353 nginx.org
```

3. Run the following command, showing an example UDP request:

```bash
dig @10.1.1.8 -p 5353 nginx.org +tcp
```

4. To show the requests successfully reaching the CoreDNS pods, show the logs using either kubectl from powershell or through Lens.  For example:

```bash
kubectl get po -n core
```

```bash
kubectl logs -n core coredns-5765cf66fd-982ls
```

