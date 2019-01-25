# docker操作命令 #

## docker-machine ##

docker-machine可以说是一个虚拟机管理工具，它可以创建、更改、删除一台虚拟机，并且可以控制这台虚拟机的启动、停止、重启。

    docker-machine -h
	
	Commands:
	  active                Print which machine is active  
							打印当前正在活动的虚拟机

	  config                Print the connection config for machine  
							打印虚拟机的连接配置

	  create                Create a machine  
							创建一台新的虚拟机

	  env                   Display the commands to set up the environment for the Docker client  
							显示连接Docker客户端的信息

	  inspect               Inspect information about a machine
							显示虚拟机的配置信息

	  ip                    Get the IP address of a machine
							获取虚拟机的IP地址
	
	  kill                  Kill a machine
							强制关闭虚拟机

	  ls                    List machines
							显示虚拟机列表								
	
	  provision             Re-provision existing machines
							重新配置现有的虚拟机

	  regenerate-certs      Regenerate TLS Certificates for a machine
							重新为虚拟机生成TLS连接的证书

	  restart               Restart a machine
							重启虚拟机

	  rm                    Remove a machine
							删除一台虚拟机

	  ssh                   Log into or run a command on a machine with SSH.
							使用SSH的方式登录虚拟机或运行命令

	  scp                   Copy files between machines
							在两台虚拟机之间复制文件

	  start                 Start a machine
							启动虚拟机

	  status                Get the status of a machine
							查看虚拟机状态

	  stop                  Stop a machine
							停止虚拟机

	  upgrade               Upgrade a machine to the latest version of Docker
							为虚拟机升级docker到最新版本

	  url                   Get the URL of a machine
							获得虚拟机的URL地址

	  version               Show the Docker Machine version or a machine docker version
							获取docker的版本号

	  help                  Shows a list of commands or help for one command


Docker提供的 Docker Quickstart Terminal 启动时，默认连接的是default虚拟机。


	                        ##         .
	                  ## ## ##        ==
	               ## ## ## ## ##    ===
	           /"""""""""""""""""\___/ ===
	      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
	           \______ o           __/
	             \    \         __/
	              \____\_______/
	
	docker is configured to use the default machine with IP 192.168.99.100
	# 默认使用default虚拟机 
	For help getting started, check out the docs at https://docs.docker.com
	
	Start interactive shell

显示所有虚拟机的状态：docker-machine ls

	$ docker-machine ls
	NAME         ACTIVE   DRIVER       STATE     URL                         SWARM			DOCKER     ERRORS
	default      *        virtualbox   Running   tcp://192.168.99.100:2376					v18.09.0
	Tensorflow   -        virtualbox   Running   tcp://192.168.99.101:2376					v18.09.0

ACTIVE中的*代表正在与当前窗口通信的虚拟机，在当前窗口执行的所有操作（比如拉取镜像）都是针对正在通信的虚拟机。

如果想要切换通信的虚拟机，可以运行 docker-machine env [vm]

	$ docker-machine env Tensorflow
	export DOCKER_TLS_VERIFY="1"
	export DOCKER_HOST="tcp://192.168.99.101:2376"
	export DOCKER_CERT_PATH="C:\Users\fangjiayao\.docker\machine\machines\Tensorflow
	"
	export DOCKER_MACHINE_NAME="Tensorflow"
	export COMPOSE_CONVERT_WINDOWS_PATHS="true"
	# Run this command to configure your shell:
	# eval $("F:\Program Files\Docker Toolbox\docker-machine.exe" env Tensorflow)

并按照给出的提示，键入eval $(docker-machine env Tensorflow)即可切换虚拟机

	$ eval $(docker-machine env Tensorflow)
	docker-machine ls
	NAME         ACTIVE   DRIVER       STATE     URL                         SWARM			DOCKER     ERRORS
	default      -        virtualbox   Running   tcp://192.168.99.100:2376					v18.09.0
	Tensorflow   *        virtualbox   Running   tcp://192.168.99.101:2376					v18.09.0

## 通过xshell访问虚拟机 ##

通过docker-machine创建了虚拟机，可以通过配置xshell等ssh连接工具方便的连接它。

docker-machine config 会显示与当前窗口通信的虚拟机的连接配置信息

	$ docker-machine config

	--tlsverify
	--tlscacert="C:\\Users\\xxx\\.docker\\machine\\machines\\default\\ca.pem"
	
	--tlscert="C:\\Users\\xxx\\.docker\\machine\\machines\\default\\cert.pem"
	
	--tlskey="C:\\Users\\xxx\\.docker\\machine\\machines\\default\\key.pem"
	-H=tcp://192.168.99.100:2376

显示当前虚拟机的连接地址是**192.168.99.100**，ssh端口号为**22**

用户名默认为**docker**，密码默认为**tcuser**。

配置完成之后即可访问docker的虚拟主机。

	Connecting to 192.168.99.100:22...
	Connection established.
	To escape to local shell, press 'Ctrl+Alt+]'.
	
	   ( '>')
	  /) TC (\   Core is distributed with ABSOLUTELY NO WARRANTY.
	 (/-_--_-\)           www.tinycorelinux.net


## docker ##

docker主要是用来对单一虚拟机内部进行操作。

通俗来讲，docker-machine就像网管一样，帮你安装电脑，开开机什么的。而docker更像是系统的管理员，可以让你往你的电脑里面安装各种软件。

1. 拉取镜像： docker pull 镜像名：版本号（不填写版本号，默认为latest）

		docker@default:~$ docker pull python:2.7.8
		2.7.8: Pulling from library/python
		a3ed95caeb02: Pull complete 
		5d3df020ecd3: Downloading [=>                                                 ]  2.153MB/59.17MB
		ecf4356ceda8: Downloading [=>                                                 ]  2.152MB/83.14MB
		525789497646: Downloading [=====>                                             ]  1.456MB/12.63MB
		ac0a3996cf90: Waiting 
		779176b4a005: Waiting 
		bff1d023a551: Waiting 
		5ed0edbedc0d: Waiting 
	

	通过该命令来拉取镜像，当前虚拟机的所有镜像可以通过docker images来查看

		docker@default:~$ docker images
		REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
		ubuntu              latest              93fd78260bd1        5 weeks ago         86.2MB


	
2. 运行镜像： docker run 镜像名
		

		docker@default:~$ docker run -it ubuntu
		root@54e2c9fa20ee:/#

	这时，镜像就已经开始运行，生成一个容器并且分配给你一个控制台，让你可以通过shell来操作运行的镜像。

	这里需要注意两个参数：

		-i，--interactive                    Keep STDIN open even if not attached
											 保持STDIN打开，即使没有连接

		-t, --tty                            Allocate a pseudo-TTY
											 分配一个虚拟控制台

		-p，--publish list                   Publish a container's port(s) to the host
											 将容器端口映射到虚拟机端口

	想要退出当前容器，在控制台输入exit即可

3. 查找镜像： docker search 镜像名

		docker@default:~$ docker search Tensorflow
		NAME                                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
		tensorflow/tensorflow               Official Docker images for the machine learn…   1248                                    
		jupyter/tensorflow-notebook         Jupyter Notebook Scientific Python Stack w/ …   101                                     
		xblaster/tensorflow-jupyter         Dockerized Jupyter with tensorflow              52                                      [OK]
		tensorflow/serving                  Official images for TensorFlow Serving (http…   32                                      
		floydhub/tensorflow                 tensorflow                                      15                                      [OK]
                                     

	
	OFFICIAL： 表示是否是由官方发布的

	AUTOMATED： 自动构建相关

## docker持久化 ##

Docker镜像重启后会发现刚刚安装的vim没了，日志也没有了，究其原因，docker容器只是作为一个运行环境存在的，它的本意并不是保存数据。
而我们肯定会希望这次的辛苦成果可以延续到下次使用，那么就需要持久化数据了。

**数据层面的持久化**

如果我们只是想要将数据保存下来，那么比较推荐的做法是将本地目录挂载到容器的目录中。这样把容器在运行过程中获得的数据保存到挂载目录上，实际上就是保存到了本地，这些数据是不会因docker镜像的重启而丢失的。

运行镜像时，添加-v参数 

	docker run -it -v /docker/default:/docker_default 镜像名
	
	-v		挂载磁盘（本地目录：挂载目录）
	
**容器的持久化**

当我们为容器安装了新的应用，或者更改了系统的配置，那么光是保存数据肯定是不够的了，那这时就需要将整个容器备份下来，供下次使用。

	docker@default:~$ docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
	2e21db4656f6        ubuntu              "/bin/bash"         47 seconds ago      Up 46 seconds                           gifted_mahavira

可以看到CONTAINER ID，提交

	docker commit 2e21db4656f6 ubuntu1
	
再看就多了刚刚新建的镜像

	docker@default:/$ docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
	ubuntu1             latest              e0d52d6c8bf8        About a minute ago   170MB
	pycharm_helpers     PY-162.1237.1       da35886ab4f4        3 weeks ago          22.2MB
	node                latest              21421bbe6a42        3 weeks ago          894MB
	ubuntu              latest              93fd78260bd1        2 months ago         86.2MB
	python              2.7.8               964c736ee415        4 years ago          801MB
	
再次运行新建的镜像，发现刚刚创建的内容全都保留了下来。

