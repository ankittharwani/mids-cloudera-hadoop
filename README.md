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
docker run --hostname=quickstart.cloudera \
           --privileged=true \
           --name=cloudera \
           -t -i -d \
           -p 8889:8889 \
           -p 8888:8888 \
           -p 7180:7180 \
           -p 8088:8088 \
           -p 8042:8042 \
           -p 19888:19888 \
           -v <host-path-to-mount-inside-container>:/media/notebooks \
           ankittharwani/mids-cloudera-hadoop:latest \
           /usr/bin/docker-quickstart
```
* Replace `<host-path-to-mount-inside-container>` with the directory on your machine which you'd want to make available in the container.

For more details, you can refer to:
> https://www.cloudera.com/documentation/enterprise/5-6-x/topics/quickstart_docker_container.html

* Once you've created the container and has been ran, you can check running status by:
```
docker ps
```



#### Login to the container and launch notebook / pyspark

* Once the container has been created, you can login to it (launch bash terminal):
```
docker exec -ti cloudera /bin/bash
```

* Once inside the terminal, you can launch notebook/pyspark by typing **pyspark** in the terminal. This will generate you a URL with a login token, which you can copy-paste in the host machine's browser.
```
[root@quickstart /]# pyspark
[TerminalIPythonApp] WARNING | Subcommand `ipython notebook` is deprecated and will be removed in future versions.
[TerminalIPythonApp] WARNING | You likely want to use `jupyter notebook` in the future
[I 20:54:18.598 NotebookApp] Writing notebook server cookie secret to /root/.local/share/jupyter/runtime/notebook_cookie_secret
[W 20:54:18.812 NotebookApp] WARNING: The notebook server is listening on all IP addresses and not using encryption. This is not recommended.
[I 20:54:18.897 NotebookApp] Serving notebooks from local directory: /media/notebooks
[I 20:54:18.897 NotebookApp] 0 active kernels
[I 20:54:18.899 NotebookApp] The Jupyter Notebook is running at: http://[all ip addresses on your system]:8889/?token=bc0ab6b91dcb2a832f11beab991bf21ed0e9bb334cbd9393
[I 20:54:18.899 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 20:54:18.902 NotebookApp]

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8889/?token=xxx
```

* You can also run a few hadoop commands to test everything is okay:
```
[root@quickstart /]# hdfs dfs -ls /
Found 5 items
drwxrwxrwx   - hdfs  supergroup          0 2016-04-06 02:26 /benchmarks
drwxr-xr-x   - hbase supergroup          0 2017-01-24 20:49 /hbase
drwxrwxrwt   - hdfs  supergroup          0 2017-01-24 20:50 /tmp
drwxr-xr-x   - hdfs  supergroup          0 2016-04-06 02:27 /user
drwxr-xr-x   - hdfs  supergroup          0 2016-04-06 02:27 /var
```

* With the current Cloudera Docker image, you should also be able to access Hue, which is a web based interface to Hive/Impala/HDFS etc. To access:

> http://localhost:8888/

You can replace **8888** with the **hue-port** configured above

Username: cloudera

Password: cloudera


#### Conda and Python Packages

* The following python packages are installed under Conda:
	* bokeh
	* cython
	* ipython
	* ipython_genutils
	* ipython-qtconsole
	* ipython-notebook
	* libpng
	* jupyter
	* mrjob
	* nltk
	* notebook
	* numpy
	* pandas
	* pip
	* scipy
	* scikit-learn
	* scikit-image
	* setuptools
	* sympy
	* wheel
	* unicodecsv
	* ujson
	* zlib

* To use the miniconda environment outside of the notebook:
```
source /opt/anaconda/bin/activate
```