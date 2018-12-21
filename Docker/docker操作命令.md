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
	
	docker is configured to use the **default** machine with IP 192.168.99.100
	For help getting started, check out the docs at https://docs.docker.com
	
	Start interactive shell

显示所有虚拟机的状态：docker-machine ls

	$ docker-machine ls
	NAME         ACTIVE   DRIVER       STATE     URL                         SWARM			DOCKER     ERRORS
	default      *        virtualbox   Running   tcp://192.168.99.100:2376				v18.09.0
	Tensorflow   -        virtualbox   Running   tcp://192.168.99.101:2376				v18.09.0

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
	default      -        virtualbox   Running   tcp://192.168.99.100:2376				v18.09.0
	Tensorflow   *        virtualbox   Running   tcp://192.168.99.101:2376				v18.09.0
