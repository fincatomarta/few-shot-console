# Action recognition and replication
This is a guide to install and run on R1SN003 the action recognition + action replication module.
N.B. If you are using the R1 LAPTOPT skip the installation step.

## Installation

```
git clone https://github.com/fbrand-new/few-shot-console.git
cd few-shot-console
docker build -t <name>
./docker_run.sh
```

## Go inside the right container
If you are working directly on the R1 LAPTOPT the image has already been installed, so open the container.

```
docker ps -a
docker exec -it <nome> bash
```
## Launching action recognition
Once inside the docker container

```
conda activate fcl_yarp

yarp conf 192.168.100.10 10000
yarp detect

python action_recognition.py

yarp connect depthImage:o action_recognition/Image:i
```
To see what the robot sees:
```
yarpview --name <nome_porta:i>
yarp connect rgbImage:o <nome_porta:i>
```
Now the action recognition module is going.

## Launching YARP ACTION PLAYER

Outside the docker container, go inside the few-shot-console repository, and make sure that it contains the files '.txt' necessary for the yarp action player.

```
yarpActionPlayer --filename configuration.ini --execute
```

## Launching robot_behaviour.py
```
python robot_behaviour.py
```

Now we need to connect the input and output ports.

```
yarp connect /action_recognition/behaviour:o actionPlayer/rpc

yarp connect /safsar/action_recognition/actions:o /action_recognition/action:i
```
Now you can test if the action recognition and action replication works. 
