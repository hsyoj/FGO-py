# AlasFpyBridge

> 在[AzurLaneAutoScript](https://github.com/LmeSzinc/AzurLaneAutoScript)中使用FGO-py!  

本readme所在目录用于存放launch和halt文件,**文件名必须是`launch`和`halt`且在Windows下需要具有`PATHEXT`中的任一扩展名,在Linux下需要具有可执行权限**  
此二文件独立于FGO-py存在,用于定义Alas会以何种方式启动与停止FGO-py,**你需要根据你自己的环境创建这些文件**  

- `launch`是启动命令,必须,无参数  
- `halt`是停止命令,可选(如无`halt`则使用默认的os.kill AlasFpyBridge创建的FGO-py进程),接收一个参数:FGO-py自身提供的pid,这意味着无论套了多少壳,halt总会接收到FGO-py最终的pid,即使FGO-py实际在另一台计算机上  

example目录下提供了些许示例,大致对应以下使用场景:  
plain - 基本的在linux机器上部署的Alas和FGO-py  
portable - 多数用户使用的portable installer安装的Alas和FGO-py  
docker - 在docker中运行FGO-py,而Alas在主机中  

你可以把本readme所在目录拷贝至其他位置方便分布式部署  
如果你像我一样在不同的机器上部署Alas和FGO-py,那么你需要在Alas可访问的位置创建包含launch(和halt)的目录并将其填入Alas中  
本人将Alas和FGO-py分别部署在两个docker容器中,容器镜像均使用对应项目现有的Dockerfile构建,那么,以下是本人实际使用的launch与halt:  

```bash
# launch
#!/bin/bash
if [ ! -e /root/.ssh/id_rsa ]; then
    mkdir -p /root/.ssh
    cp /app/sshkey/id_rsa /root/.ssh/id_rsa
    chmod 600 /root/.ssh/id_rsa
fi
ssh hgjazhgj@host.docker.internal -o StrictHostKeyChecking=no "docker run -v D:/FGO-py:/app/FGO-py --name fgo-py -e NO_COLOR=1 -e ADB_SERVER_SOCKET=tcp:host.docker.internal:5037 -i --rm hgjazhgj/fgo-py"

# halt
#!/bin/bash
ssh hgjazhgj@host.docker.internal -o StrictHostKeyChecking=no "docker kill fgo-py"
```

主机上已(用某个进程管理器)运行`adb -a nodaemon server start`以迎合模拟器只监听localhost
以及Alas的容器以以下命令启动:  

```powershell
docker run -v ${PWD}:/app/AzurLaneAutoScript -v ${PWD}/../FGO-py/deploy/AzurLaneAutoScript:/app/fpydeploy -v $env:USERPROFILE/.ssh/id_rsa:/app/sshkey/id_rsa:ro -p 22267:22267 --name alas -it --rm hgjazhgj/alas
```

此时Alas中填写的启动路径将是`/app/fpydeploy`  
