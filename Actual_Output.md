
HOST_A: 192.168.100.226
Host_B: 192.168.100.109

![image](https://github.com/chrahul/testing-Overlay-Network-in-OCI/assets/14847377/8fb4c5ca-6064-435f-9bdf-aed874a2ed38)


---

#### Step 1: Initialize Docker Swarm on host-a:

```bash
docker swarm init --advertise-addr=192.168.100.226
```

**Output:**
```
Swarm initialized: current node (im5rq7mzck7sozhr9l5hownqk) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-1mf2vjpk7ibd4w4wte3bq8iulop962r9t1m1bhiut8j4jbuoo7-ekhvyafmpxrj94x0d7825qls9 192.168.100.226:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

---

#### Step 2: Create an overlay network:

```bash
docker network create -d overlay my-overlay
```

**Output:**
```
04ml6bdhfqtwu5vcckoo93nyg
```

---

#### Step 3: Deploy a service on the overlay network:

```bash
docker service create --name my-nginx --network my-overlay --replicas 1 --publish published=8080,target=80 nginx:latest
```

**Output:**
```
4qpqmnraeaoj   my-nginx   replicated   1/1        nginx:latest   *:8080->80/tcp
```

---

#### Step 4: Access the service from host-b:

```bash
curl http://192.168.100.226:8080
```

**Output:**
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

---

#### Conclusion:

You have successfully tested Docker overlay networking by deploying an Nginx service on a Swarm overlay network and accessing it from a different host within the same network.

---

### Additional Notes:
- Ensure that both hosts have proper network connectivity and firewall rules allowing communication on the required ports.
- Replace `<host-a-IP>` with the actual IP address of host-a.
- Make sure Docker is installed and running on both hosts before proceeding with the steps.
- Troubleshoot any connectivity issues by checking Docker logs, network configurations, and firewall settings.
