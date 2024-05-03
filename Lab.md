Here's a step-by-step document outlining the process of testing Docker overlay networking:

### Testing Docker Overlay Networking

#### Pre-requisites:
1. Two Linux machines (host-a and host-b) connected to the same network/subnet.
2. Docker installed on both machines.
3. Basic knowledge of Docker commands.

#### Steps:

**1. Initialize Docker Swarm on host-a:**
```bash
docker swarm init --advertise-addr=<host-a-IP>
```
This command initializes Docker Swarm on host-a and makes it the Swarm manager.

**2. Create an overlay network:**
```bash
docker network create -d overlay my-overlay
```
This command creates an overlay network named "my-overlay" for communication between services in the Swarm.

**3. Deploy a service on the overlay network:**
```bash
docker service create --name my-nginx --network my-overlay --replicas 1 --publish published=8080,target=80 nginx:latest
```
This command deploys an Nginx service called "my-nginx" on the overlay network "my-overlay". It publishes port 8080 on the host and routes traffic to port 80 in the container.

**4. Access the service from host-b:**
```bash
curl http://<host-a-IP>:8080
```
This command tests the connectivity to the Nginx service from host-b using the IP address of host-a. If successful, it should display the default Nginx welcome page.

#### Verification:

**1. Verify service deployment:**
```bash
docker service ls
```
This command lists all the services running in the Swarm. Ensure that the "my-nginx" service is listed and has 1 replica.

**2. Verify network creation:**
```bash
docker network ls
```
This command lists all the networks created in Docker. Ensure that "my-overlay" is listed with the overlay driver and is marked as part of the Swarm.

**3. Verify network details:**
```bash
docker network inspect my-overlay
```
This command provides detailed information about the "my-overlay" network, including its configuration and connected containers.

#### Conclusion:
You have successfully tested Docker overlay networking by deploying an Nginx service on a Swarm overlay network and accessing it from a different host within the same network.

### Additional Notes:
- Ensure that both hosts have proper network connectivity and firewall rules allowing communication on the required ports.
- Replace `<host-a-IP>` with the actual IP address of host-a.
- Make sure Docker is installed and running on both hosts before proceeding with the steps.
- Troubleshoot any connectivity issues by checking Docker logs, network configurations, and firewall settings.
