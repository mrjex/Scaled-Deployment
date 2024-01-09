# Dentanoid Deployment (Advanced)

This repo attempts to address the concerns of availability and performance for the system concidering that there will be high demand on launch.

The measures taken in the process of deployment are presented here.

## Docker Swarm Mode <img src="https://substack-post-media.s3.amazonaws.com/public/images/ea0bb372-1b18-4abe-acb5-456035630fb2_269x201.png" width="40px">

A Docker Swarm is a group of either physical or virtual machines that are running the Docker application and that have been configured to join together in a cluster.

## Perks
Below are some perks that are achieved thanks to the nature of swarm mode

### Availabiity

<img src="https://i.imgur.com/PijjK8q.png" width="200px">

Firstly, docker swarms allow for high availability by specifying the **--replicas** option, this option allows for spawning multiple instances of the same program  in the context of a single node, this means that losing a single instance of the service does not lead to downtime. \
\
More importantly, swarm mode allows for node creation on **different host machines** with one node being chosen as the manager. This has huge benefits since now an entire machine can go offline with little to no impact on the overall availability of the system. 

This can be seen in action in the image above which shows the Dentanoid API being scaled on a swarm consisting of a node on the left acting as the manager and the node on the right which is running on a seperate machine on the same network acting as a worker node.

### Performance
<img src="https://www.cloud4u.com/upload/medialibrary/5a6/0_CCK15OF3DizmOITk.png">
<img src="https://media.discordapp.net/attachments/1011556085221556248/1193374170357186672/image.png" width="220px" />

Swarm mode has a built-in **load balancer** which utilizes the round robin algorithm to evenly distribute the load across the nodes, this ensures optimal performance by potentially decreasing latency.

