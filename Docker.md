## Docker 镜像命令

* **查看本机所有镜像 ** 

    > * docker images
    >   * -a **列出所有镜像**
    >   * -q **只显示镜像ID**

* **搜索镜像**
  
  > * docker search [image]
  >   * --filter=STARS=3000   **指定最小star数**
  
* **下载镜像**

    > * docker pull [image] [:tag]  (指定版本)

* **删除指定镜像**

    > * docker rmi -f [镜像ID]        
    >
    > * docker rmi -f $(docker images -aq)  **删除所有容器**

* **修改镜像名称**

  > * docker tag [imagename] [newimagename]





## Docker 容器命令

*   **创建 并启动容器**

  > * docker run  
  >   * --name="Name"   **指定创建容器的名字**
  >   * -d  **后台方式运行**
  >   * -it  **使用交互方式运行,进入容器查看内容**
  >   * -p  **指定主机端口**

*  **列出正在运行的容器 (查找容器)**

  >* docker ps 
  >    * -a   **列出所有运行过的容器(包括正在运行的容器)**

* **退出容器**

  >* exit  **退出 且停止容器**
  >
  >* Ctrl + P + Q   **退出 但不停止容器**

* **删除容器**

  > * docker rm [容器ID]   **删除指定容器**
  >
  > * docker rm -f $(docker ps -aq)  **强制删除所有容器(包括正在运行)**

* **启动 停止容器**

  > * docker start [容器ID]  
  > * docker restart [容器ID]
  > * docker stop [容器ID]
  > * docker kill [容器ID]

* **进入容器**

  > * docker exec [容器ID]  /bin/bash **进入容器 开启一个新的终端**
  > * docker attach [容器ID]  /bin/bash **进入容器 正在执行的终端**

* **查看容器信息**

  > * docker inspect [容器ID]   **可查看挂载 同步目录信息等**







## Docker 生成镜像

* **从容器内拷贝文件到主机**

  > * docker cp [容器ID]:[文件]  [主机目标目录] 

* **操作过的容器制作成镜像 (退出容器后制作)**

  > * docker commit -m="描述信息"  -a="作者"  [容器ID]  目标镜像名:[tag]







## Docker 数据卷

* **用命令挂载目录 自动同步信息**

  > * docker run -it  -v  [主机目录]  [容器目录]  镜像

* **创建镜像时 指定数据卷 挂在在默认目录**

  > * docker build -f [dockerfile绝对地址] -t  镜像名[;tag]     [存储地址]
  >
  > ```shell
  > #  dockerfile 文件内容 (挂在主机目录默认在 /var/lib/docker/volume/..)
  > FROM  centos
  > 
  > VOLUME ["volume01", "volume02"]
  > 
  > CMD echo "---end---"
  > 
  > CMD /bin/bash
  > ```

* **多个容器间 数据自动同步**

  > * docker run -it --name docker02  --volumes-from docker01 [镜像]







## DockerFile 制作镜像

* **制作镜像**

  > **１： 编写dockfile文件**
  >
  > ```shell
  > FROM centos
  > MAINTAINER  wukong<2466913104@qq.com>
  > 
  > ENV MYPATH  /usr/local       #　配置环境目录
  > WORKDIR   $MYPATH　　# 工作目录
  > 
  > RUN sudo apt-get install -y vim  net-tools          # 执行命令
  > 
  > EXPOSE  80  # 暴露端口
  > 
  > CMD echo $MYPATH
  > CMD /bin/bash   #终端位置
  > ```
  >
  > **2：生成镜像**
  >
  > ```
  > docker build -f mydockerfile-centos  -t  mycentos ./
  > ```



* **dockerfile命令**

  > * CMD ["ls","-a"]     **执行终端命令（run 时不可追加）**
  > * ENTRYPOINT ["ls", "-a"]  **执行终端命令（run时可追加命令）**



* **发布镜像**

  > * docker login 
  >   * -u [用户名]  **直接输入用户名**
  > * docker push [镜像命名] 