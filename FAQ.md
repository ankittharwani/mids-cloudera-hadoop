# Docker FAQ

## What is Docker? 

Docker is a container management platform for automating deployments of re-usable environemnets. 

## What are Containers? Containers vs VM

Containers are a type of virtualization. They virtualize environments at the operating system level by sharing the base libraries. This removes the need for boot drives and hardware interfaces for each environment. This makes a container use much less resources than a virtual machine (VM) which can make environments more efficient because lack of duplication of resources.

![alt text](http://zdnet2.cbsistatic.com/hub/i/r/2017/05/08/af178c5a-64dd-4900-8447-3abd739757e3/resize/770xauto/78abd09a8d41c182a28118ac0465c914/docker-vm-container.png "Container vs VM")

## Why Docker?

Using Docker we can keep a fresh deployment of our Hadoop/Jupyter/Spark environment readily avaliable. Taking around 2 minutes to remove and launch a new environment with the same initial configuration everytime.

## Things to know

In the container for W261 we use docker-compose to build our container. Let's review the configuration file docker-compose.yml in its current state at the time of writing

```
version: '2'
services:
  quickstart.cloudera:
    image: ankittharwani/mids-cloudera-hadoop:latest
    hostname: quickstart.cloudera
    privileged: true
    command: bash -c "/root/startup.sh; /usr/bin/docker-quickstart; conda install -c conda-forge mrjob=0.5.5"
    ports:
      - "8887:8888"   # Hue server
      - "8889:8889"   # jupyter
      - "10020:10020" # mapreduce job history server
      - "8022:22"     # ssh
      - "7180:7180"   # Cloudera Manager
      - "11000:11000" # Oozie
      - "50070:50070" # HDFS REST Namenode
      - "50075:50075" # hdfs REST Datanode
      - "8088:8088"   # yarn resource manager webapp address
      - "19888:19888" # mapreduce job history webapp address
      - "8983:8983"   # Solr console
      - "8032:8032"   # yarn resource manager access
      - "8042:8042"   # yarn node manager
      - "60010:60010" # hbase
    tty: true
    stdin_open: true
    volumes: 
      - /home/pryan/Projects/ds@b/w261/midsw261/hw-pjryan126:/media/notebooks
```

- version: this item says use v2 syntax
- services: list of containers
  - quickstart.cloudera: the name of a container, the label being quickstart.cloudera
    - image: use this base container
    - hostname: DNS name for the container
    - privledged: allow access to other machines such as the local machine
    - commands: run this commands on start
    - ports: map ports so that services running on the container are accessible from the local computer
      - remote port:local port
    - tty: allow a shell to be initiated
    - stdin_open: allow interactivity with the shell
    - volumes: location to map from local computer to the docker container so they can share. 
      - /local/path:/media/notebook

If we review the bash scripts `startup.sh` we can see that the jupyter notebook is launched from the `/media/notebook` directory. This is very important for our deployment.

## General Issues

### Hostname mapping

Apply the `quickstart.cloudera` alias for `127.0.0.1` aka `localhost`
- Linux & Mac
  1. Open Terminal
  2. Open hostfile by running `sudo nano /etc/hosts`
  3. Append the following line, then save: `127.0.0.1    quickstart.cloudera`
  4. Refresh DNS with `sudo killall -HUP mDNSResponder`
- Windows:
  1. Open notepad as administrator (otherwise you'll not be able to save the file)
  2. Open `C:\Windows\System32\drivers\etc\hosts` in notepad.  Note the file has no extension
  3. Append the following line, then save: `127.0.0.1    quickstart.cloudera`
  4. Refresh DNS by running `ipconfig /flushdns` in command prompt or powershell
  
### Minimum System Requirements for MIDS W261 Cloudera Hadoop Container

Docker needs 2 CPUs and 4 GB of RAM to ensure resource managers don't crash during normal operation. 
- Linux
  1. By default Docker shares the same resources as the local computer.
- Windows
  1. Right click Docker in the notification area
  2. Click Settings
  3. Click Advanced
  4. Slide Memory to 4096 MB
- Mac OS
  1. Click Docker in the clock(?) area
  2. Click Settings
  3. Click Advanced
  4. Slide Memory to 4096 MB
  
## Linux Issues

## Windows Issues

- Windows 10 Pro/Education is required to run Docker on Windows. A free license of Windows 10 Education is avaliable to all students through [UCB Software Central](https://software.berkeley.edu/operating-systems#Microsoft)

## Mac Issues

- Macs require a computer capable of virtualization to test this run `sysctl kern.hv_support` in a terminal.
  - If 1 then good to go
  - If 0 then you need a new computer
