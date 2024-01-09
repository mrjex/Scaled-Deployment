# Dentanoid Deployment (Advanced)

This repo attempts to address the concerns of availability and performance in the distributed system especially concidering that there will be high demand on launch.

The measures taken in the process of deployment are presented in this repo.

## Docker Swarm Mode <img src="https://substack-post-media.s3.amazonaws.com/public/images/ea0bb372-1b18-4abe-acb5-456035630fb2_269x201.png" width="40px">

A Docker Swarm is a group of either physical or virtual machines that are running the Docker application and that have been configured to join together in a cluster.

## Perks
Below are some perks that are achieved thanks to the nature of swarm mode

### Availability

<img src="https://i.imgur.com/PijjK8q.png" width="600px">

Docker swarms allow for high availability by specifying the **--replicas** option, this option allows for spawning multiple instances of the same program  in the context of a single node, this means that losing a single instance of the service does not lead to downtime. \
\
More importantly, swarm mode allows for node creation on **different host machines** with one node being chosen as the manager. This has huge benefits since now an entire machine can go offline with little to no impact on the overall availability of the system. 

This can be seen in action in the image above which shows the Dentanoid API being scaled on a swarm consisting of a node on the left acting as the manager and the node on the right which is running on a seperate machine on the same network acting as a worker node.

### Performance
<img src="https://www.cloud4u.com/upload/medialibrary/5a6/0_CCK15OF3DizmOITk.png" width="500px">
<img src="https://media.discordapp.net/attachments/1011556085221556248/1193374170357186672/image.png" width="320px" />

Swarm mode has a built-in **load balancer** which utilizes the round robin algorithm to evenly distribute the load across the nodes, this ensures optimal performance by decreasing network latency.

## Steps to Deploy and Scale
- Create the swarm on the manager host by using the following command

```
docker swarm init
```
- Then different hosts can be added to the swarm as worker nodes using the output of the first command (usually containing a unique token to that swarm)

an example would be:
```
docker swarm join \
  --token  SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
  192.168.99.100:2377
```

- Finally the service can be started across the swarm (can be scaled to liking using **--replicas**)

```
docker service create --name API --replicas 6 -p 3000:3000 dentanoid-dentist:1.0
```

Navigating to `http://<HOST_IP>:3000` will now lead to a response from different nodes depending on the load balancing.

The result is an evenly distributed swarm of containers serving the same purpose

<img src="https://i.imgur.com/PijjK8q.png" width="600px">


## Deployment Diagram
The diagram below is a general overview of how the system can be deployed using docker swarms
<img src="https://i.imgur.com/TiU5yZL.png" width="650px" />

