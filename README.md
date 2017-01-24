## MIDS W261 - Cloudera Hadoop on Docker

---
This docker image consists of the Cloudera QuickStart image extended with miniconda, important python packages and Jupyter notebook configured with pyspark.

### Running this image on your machine

#### Install Docker Engine
* Before we start using the image, please make sure the Docker Engine is installed on your machine.
* For instructions installing docker engine, refer to the link below:
> https://docs.docker.com/engine/installation/

* You can just stop at Step 1, i.e. "Install and Run Docker for Mac"

#### Pull the Docker Image
* Once you have the Docker Engine up and running, you can pull the **mids-cloudera-hadoop** image.

* You can navigate to the image at the Docker Hub link below:
> https://hub.docker.com/r/ankittharwani/mids-cloudera-hadoop/

* Refer to the Tags to find the latest image - usually it is the **latest** tag itself.

* On your machine, run the following command to pull the image:
```
docker pull ankittharwani/mids-cloudera-hadoop
```

#### Create a Docker container with the pulled image
* Once you have the Docker image pulled, you can create a container with the following command:

```
docker run --hostname=quickstart.cloudera --privileged=true --name=cloudera -t -i -d -p <notebook-port>:8889 -p <hue-port>:8888 -p <cloudera-manager-port>:7180 -v //Users/ankittharwani/Work/MIDS/Extras:/media/notebooks ankittharwani/mids-cloudera-hadoop:latest /usr/bin/docker-quickstart
```

